<script setup>
    import { defineProps, defineEmits } from 'vue'
    const name = 'Input';
    const emit = defineEmits();
    const props = defineProps({
            value: {
                type: Number,
                default: ''
            },
            selected: {
                type: String,
                required: true
            }
        }
    )
</script>

<template>
    <div class="currency-input">
        <input type="number" :value="props.value" @input="emit('update:value', $event.target.value)" placeholder="Inserieci il valore">
        <select :value="props.selected" @change="emit('update:selected', $event.target.value)">
            <option value="EUR">Euro (EUR)</option>
            <option value="USD">Dollaro (USD)</option>
            <option value="GBP">Sterlina (GBP)</option>
        </select>
    </div>
</template>

<style scoped lang="scss">
    .currency-input {
        display: flex;
        gap: 10px;
        align-items: center;
        input {
            width: 120px;
            padding: 5px;
        }
        select {
            padding: 5px;
        }
    }
</style>
