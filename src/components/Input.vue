<script setup>
    import { defineProps, defineEmits } from 'vue'
    const name = 'Input';
    const emit = defineEmits();
    const props = defineProps({
            value: {
                type: Number,
                default: 0
            },
            selected: {
                type: String,
                required: true
            },
            options: {
                type: Array,
                required: true
            }
        }
    )
</script>

<template>
    <div class="currency-input">
        <input type="number" :value="value" @input="emit('update:value', $event.target.value)" placeholder="Inserieci il valore">
        <select :value="selected" @change="emit('update:selected', $event.target.value)">
            <!-- <option v-for="option in options" :key="option" :value="option">{{ option }}</option> -->
            <option v-for="([code, name]) in options" :key="code" :value="code">{{ code }} - {{ name }}</option>
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
