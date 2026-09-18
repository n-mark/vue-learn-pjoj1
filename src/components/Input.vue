<script setup>
const props = defineProps(["value"]);
const emit = defineEmits(['update:value'])
</script>

<template>
    <input class="input" :value="props.value" @input="emit('update:value', $event.target.value)">
</template>

<style scoped>
.input {
    background-color: var(--color-bg-card);
    box-shadow: 1px 2px 4px 0 var(--color-bg-main);
    border-radius: 10px;
    width: 260px;
    border: none;
    height: 53px;
    font-family: var(--font-family);
    font-weight: 400;
    font-size: 18px;
    color: var(--color-primary);
    padding: 15px 20px;
}

.input::placeholder {
    color: var(--color-text-placeholder);
}
</style>