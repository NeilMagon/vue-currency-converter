<script setup>
    import { ref } from 'vue'
    import Input from './components/Input.vue'
    const number = ref('')
    const valuta = ref('EUR')
</script>

<template>
    <div class="my-cd">
        <h1>Convertitore di value</h1>
        <Input :value="number" :selected="valuta" @update:value="number = $event" @update:selected="valuta = $event"/>
    </div>
</template>

<style scoped  lang="scss">
.my-cd{
  text-align: center;
  margin: 20px;
}
</style>
