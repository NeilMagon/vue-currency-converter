<script setup>
    import { ref, onMounted } from 'vue'
    import axios from 'axios'
    import Input from './components/Input.vue'
    const number = ref(0)
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
</script>

<template>
    <div class="my-cd">
        <h1>Convertitore di value</h1>
        <Input :value="number" :options="options" :selected="valuta1" @update:value="number = $event" @update:selected="valuta1 = $event"/>
        <Input :value="number" :options="options" :selected="valuta2" @update:value="number = $event" @update:selected="valuta2 = $event"/>
    </div>
</template>

<style scoped  lang="scss">
.my-cd{
  text-align: center;
  margin: 20px;
}
</style>
