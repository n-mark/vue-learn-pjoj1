<script setup>
import IconCloud from '../icons/IconCloud.vue';
import IconSun from '../icons/IconSun.vue';
import IconRain from '../icons/IconRain.vue';
import { computed } from "vue";

const { weatherCode, temperature, date, isActive } = defineProps({
    weatherCode: Number,
    temperature: Number,
    date: String,
    isActive: Boolean
});

const iconColor = computed(() => { return isActive ? 'var(--color-primary-inverted)' : 'var(--color-primary)' });

</script>

<template>
    <button class="day-card" :class="{ active: isActive }">
        <IconSun v-if="weatherCode <= 1003"
            :color="iconColor" />
        <IconCloud v-if="weatherCode >= 1006 && weatherCode < 1063"
            :color="iconColor" />
        <IconRain v-if="weatherCode >= 1063"
            :color="iconColor" />
        <div class="day-card__day">
            {{ new Date(date).toLocaleDateString('ru-RU', { weekday: "short" }) }}
        </div>
        <div class="day-card__temp">
            {{ temperature }} °C
        </div>
    </button>
</template>

<style scoped>
.day-card {
    color: var(--color-primary);
    padding: 20px 24px;
    background-color: var(--color-bg-card);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 15px;
    border-radius: 10px;
    box-shadow: 1px 2px 4px 0px #222831;
    border: none;
    width: 100%;
}

.day-card:not(.active):hover {
    background-color: #3a434f;
}

.active {
    background-color: var(--color-primary);
    color: var(--color-primary-inverted);
}

.day-card__day {
    font-size: 20px;
}

.day-card__temp {
    font-size: 20px;
    font-style: normal;
    font-weight: 700;
}
</style>