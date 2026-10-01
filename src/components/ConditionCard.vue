<script setup>
import IconCloud from '../icons/IconCloud.vue';
import IconSun from '../icons/IconSun.vue';
import IconRain from '../icons/IconRain.vue';

const { weatherCode, temperature, date } = defineProps({
    weatherCode: Number,
    temperature: Number,
    date: String
});


</script>

<template>
    <button class="day-card">
        <IconSun v-if="weatherCode == 1000" />
        <IconCloud v-if="weatherCode == 1003" />
        <IconRain v-if="weatherCode == 1009" />
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

.day-card:hover {
    background-color: #3a434f;
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