
  
<script setup>
    import { ref, watch, onMounted } from 'vue';
    import ApexCharts from 'apexcharts';
    import axios from 'axios';
    
    const props = defineProps({
        valuta1: String,
        valuta2: String,
    });
    
    const chart = ref(null);
    
    const fetchChartData = async () => {
        const today = new Date().toISOString().split('T')[0];
        const lastWeek = new Date();
        lastWeek.setDate(lastWeek.getDate() - 7);
        const startDate = lastWeek.toISOString().split('T')[0];
    
        try {
        const response = await axios.get(
            `https://api.frankfurter.app/${startDate}..${today}?from=${props.valuta1}&to=${props.valuta2}`
        );
    
        const dates = Object.keys(response.data.rates);
        const values = dates.map((date) => response.data.rates[date][props.valuta2]);
    
        return { dates, values };
        } catch (error) {
        console.error('Error fetching chart data:', error);
        return { dates: [], values: [] };
        }
    };
    
    const createChart = async () => {
        const chartElement = document.querySelector('#chart');
        if (chartElement) chartElement.innerHTML = ''; // Pulisci il contenitore
    
        const data = await fetchChartData();
    
        const options = {
        chart: {
            type: 'line',
            height: 350,
        },
        series: [
            {
            name: `${props.valuta1} to ${props.valuta2}`,
            data: data.values,
            },
        ],
        xaxis: {
            categories: data.dates,
        },
        colors: ['#80e750'],
        title: {
            text: `Exchange Rate: ${props.valuta1} to ${props.valuta2}`,
            align: 'center',
        },
        };
    
        chart.value = new ApexCharts(document.querySelector('#chart'), options);
        chart.value.render();
    };
    
    onMounted(createChart);
    
    watch([() => props.valuta1, () => props.valuta2], createChart);
</script>
  
<template>
    <div id="chart"></div>
</template>

<style scoped>
  #chart {
    max-width: 800px;
    margin: 0 auto;
    margin-top: 20px;
  }
</style>