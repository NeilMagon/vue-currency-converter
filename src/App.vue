<script setup>
    import { ref, onMounted } from 'vue'
    import axios from 'axios'
    import Input from './components/Input.vue'
    const number1 = ref(0)
    const number2 = ref(0)
    const valuta1 = ref('EUR')
    const valuta2 = ref('USD')
    const options = ref([])
    console.log(options)
    onMounted(async () => {
        try {
            const response = await axios.get("https://api.frankfurter.app/currencies");
            options.value = Object.entries(response.data); 
        } catch (error) {
            console.error("Errore nel recupero delle valute:", error);
        }
    });
    const convertCurrency1to2 = async () => {
        try {
            const response = await axios.get(`https://api.frankfurter.app/latest?amount=${number1.value}&from=${valuta1.value}&to=${valuta2.value}`);
            number2.value = response.data.rates[valuta2.value];
        } catch (error) {
            console.error("Errore nella conversione della valuta:", error);
        }
    };
    const convertCurrency2to1 = async () => {
        try {
            const response = await axios.get(`https://api.frankfurter.app/latest?amount=${number2.value}&from=${valuta2.value}&to=${valuta1.value}`);
            number1.value = response.data.rates[valuta1.value];
        } catch (error) {
            console.error("Errore nella conversione della valuta:", error);
        }
    };
    const updateValue1 = (value) => {
        number1.value = value;
        convertCurrency1to2();
    }
    const updateValue2 = (value) => {
        number2.value = value;
        convertCurrency2to1();
    }
    const updateValuta1 = (selected) => {
        valuta1.value = selected;
        convertCurrency1to2();
    }
    const updateValuta2 = (selected) => {
        valuta2.value = selected;
        convertCurrency2to1();
    }
    // const valutaDisabled = (valuta, selectedValuta) => {
    //     return valuta === selectedValuta
    // }   
</script>

<template>
    <div class="my-cd">
        <h1>Convertitore di valute</h1>
        <Input :value="number1" :options="options" :selected="valuta1" :is-disabled="valuta2" @update:value="updateValue1" @update:selected="updateValuta1"/>
        <Input :value="number2" :options="options" :selected="valuta2" :is-disabled="valuta1" @update:value="updateValue2" @update:selected="updateValuta2"/>
    </div>
</template>

<style scoped  lang="scss">
.my-cd{
  text-align: center;
  margin: 20px;
}
</style>
