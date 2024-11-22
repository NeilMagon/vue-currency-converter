<script setup>
    import { defineProps, defineEmits } from 'vue'
    const props = defineProps({
            // Valore di deafult
            value: {
                type: Number,
                default: 0
            },
            // Valuta selezionata
            selected: {
                type: String,
                required: true
            },
            // Lista delle valute
            options: {
                type: Array,
                required: true
            },
            // Valuta disabilitata se già selezionatain un al'tra select
            isDisabled: {
                type: String,
                required: true
            }
        }
    )
    const name = 'Input';
    // Funzioni per aggiornare i valori con emit
    const emit = defineEmits(['update:value', 'update:selected']);
    // const updateValue = (event) => {
    //     emit('update:value', parseFloat(event.target.value) || 0);
    // };
    const updateValue = (event) => {
    const newValue = parseFloat(event.target.value) || 0; // Conversione del valore a numero
        emit('update:value', newValue);
    };
    const updateSelected = (event) => {
        emit('update:selected', event.target.value);
    };
    // Funzione per controllare se la valuta è disabilitata
    const isDisabled = (valuta) => {
        return valuta === props.isDisabled;
    }
</script>

<template>
    <div class="currency-input">
        <input type="number" :value="value" @input="updateValue" placeholder="Inserisci il valore">
        <select :value="selected" @change="updateSelected">
            <!-- <option v-for="option in options" :key="option" :value="option">{{ option }}</option> -->
            <option v-for="([code, name]) in options" :key="code" :value="code" :disabled="isDisabled(code)">{{ code }} - {{ name }}</option>
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
