<script setup>
// defineOptions({
//     inheritAttrs: false
// })
</script>

<template>
    <button class="button">
        <slot />
        <!-- {{ $attrs['class'] }} -->
    </button>
</template>

<style scoped>
.button {
    border: none;
    border-radius: 10px;
    background: var(--gradient);
    padding: 14px;
    font-family: var(--font);
    font-size: 20px;
    font-weight: 600;
    color: var(--primary);
    height: 53px;
    cursor: pointer;
    display: flex;
    gap: 9px;
    align-items: center;
    width: 100%;
    justify-content: center;
}

.button:hover {
    background: var(--gradient-inverted);
}
</style>
