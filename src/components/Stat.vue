<script setup>
const { label = "Не задано", stat } = defineProps({
    label: String,
    stat: String
});
</script>

<template>
    <div class="stat">
        <div class="stat-name">{{ label }}</div>
        <div class="stat-value">{{ stat }}</div>
    </div>
</template>

<style scoped>
.stat {
    font-size: 28px;
    display: flex;
    justify-content: space-between;
    gap: 16px;
    width: 415px;
}

.stat-name {
    font-weight: 700;
    text-transform: uppercase;
}

.stat-value {
    font-weight: 50;
    width: 30%;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    flex-direction: row;
}
</style>