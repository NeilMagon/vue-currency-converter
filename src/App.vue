<script setup>
    import { ref, onMounted } from 'vue'
    import axios from 'axios'
    import Input from './components/Input.vue'
    // Valore di deafault prima input
    const number1 = ref(0)
    // Valore di deafault seconda input
    const number2 = ref(0)
    // Valuta selezionata di default prima select
    const valuta1 = ref('EUR')
    // Valuta selezionata di default seconda select
    const valuta2 = ref('USD')
    // Lista delle valute
    const options = ref([])
    console.log(options)
    // Funzione axios per recuperare la lista delle valute all'avvio
    onMounted(async () => {
        try {
            const response = await axios.get("https://api.frankfurter.app/currencies");
            options.value = Object.entries(response.data); 
        } catch (error) {
            console.error("Errore nel recupero delle valute:", error);
        }
    });
    // Funzioni per convertire valute in base se l'utente modifica la prima o la seconda input
    // Funzione axios per convertire la prima valuta alla seconda 
    const convertCurrency1to2 = async () => {
        try {
            const response = await axios.get(`https://api.frankfurter.app/latest?amount=${number1.value}&from=${valuta1.value}&to=${valuta2.value}`);
            number2.value = response.data.rates[valuta2.value];
        } catch (error) {
            console.error("Errore nella conversione della valuta:", error);
        }
    };
    // Funzione axios per convertire la seconda valuta alla prima 
    const convertCurrency2to1 = async () => {
        try {
            const response = await axios.get(`https://api.frankfurter.app/latest?amount=${number2.value}&from=${valuta2.value}&to=${valuta1.value}`);
            number1.value = response.data.rates[valuta1.value];
        } catch (error) {
            console.error("Errore nella conversione della valuta:", error);
        }
    };
    // Funzioni per aggiornare i valori quando si modifica la prima o la seconda input
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
</script>

<template>
    <div class="my-cd">
        <h1>Convertitore di valute</h1>
        <!-- Prima input e select -->
        <Input :value="number1" :options="options" :selected="valuta1" :is-disabled="valuta2" @update:value="updateValue1" @update:selected="updateValuta1"/>
        <!-- Seconda input e select -->
        <Input :value="number2" :options="options" :selected="valuta2" :is-disabled="valuta1" @update:value="updateValue2" @update:selected="updateValuta2"/>
    </div>
</template>

<style scoped  lang="scss">
.my-cd{
  text-align: center;
  margin: 20px;
}
</style>
