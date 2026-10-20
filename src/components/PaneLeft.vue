<script setup>
import { computed, watch } from 'vue';
import IconLocation from '../icons/IconLocation.vue';
import IconSun from '../icons/IconSun.vue';
import IconCloud from '../icons/IconCloud.vue';
import IconRain from '../icons/IconRain.vue';

const { data, idx = 0 } = defineProps({
    data: Object,
    idx: Number
});

const displayData = computed(() => {
    if (!data) {
        return undefined;
    }

    console.log(data);

    return {
        dayOfWeek: capitalize(new Date(data.forecast.forecastday[idx].date).toLocaleDateString('ru-RU', { weekday: "long" })),
        dateFormatted: formatDate(new Date(data.forecast.forecastday[idx].date)),
        cityName: data.location.name,
        weatherCode: data.forecast.forecastday[idx].day.condition.code,
        temperature: data.forecast.forecastday[idx].day.maxtemp_c,
        condition: data.forecast.forecastday[idx].day.condition.text,
    }
});

function capitalize(str) {
    if (!str) return str;
    const [first, ...rest] = Array.from(str);
    return first.toUpperCase() + rest.join('');
}

const formatDate = (date) =>
    new Intl.DateTimeFormat('ru-RU', {
        day: 'numeric',
        month: 'long',
        year: 'numeric',
    }).format(date);

</script>

<template>
    <div v-if="data" class="container">
        <div class="upper">
            <h1>{{ displayData.dayOfWeek }}</h1>
            <p>{{ displayData.dateFormatted }}</p>
            <div class="location">
                <IconLocation :size=80 />
                <div class="city-name">{{ displayData.cityName }}</div>
            </div>
        </div>
        <div class="bottom">
            <div class="icon-container">
                <IconSun v-if="displayData.weatherCode <= 1003" :size=160 />
                <IconCloud v-if="displayData.weatherCode >= 1006 && displayData.weatherCode < 1063"
                    :size=160 />
                <IconRain v-if="displayData.weatherCode >= 1063" :size=160 />
            </div>
            <div class="temperature">{{ displayData.temperature }} °C</div>
            <div class="weather-description">{{ displayData.condition }}</div>
        </div>
    </div>
</template>

<style scoped>
.container {
    width: 85%;
    height: 87%;
    /* background-color: rgba(0, 255, 255, 0.301); */
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.container h1 {
    margin: 0 0 10px 0;
    color: #FFF;
    font-family: Montserrat;
    font-size: 37px;
    font-style: normal;
    font-weight: 700;
    line-height: normal;
}

.container p {
    margin: 0 0 20px 0;
    color: #FFF;
    font-family: Montserrat;
    font-size: 22px;
    font-style: normal;
    font-weight: 500;
    line-height: normal;
}

.upper {
    width: 100%;
    height: 25%;
    /* background-color: rgba(128, 0, 128, 0.288); */
    display: block;
}

.bottom {
    width: 100%;
    height: 40%;
    /* background-color: rgba(240, 248, 255, 0.315); */
}

.icon-container {
    /* margin: 20px; */
    /* margin-bottom: 25px; */
    margin-left: 18px;
    height: 100px;
}

.temperature {
    color: #FFF;
    font-family: Montserrat;
    font-size: 50px;
    font-style: normal;
    font-weight: 700;
    line-height: normal;
}

.weather-description {
    color: #FFF;
    font-family: Montserrat;
    font-size: 30px;
    font-style: normal;
    font-weight: 700;
    line-height: normal;
}

.location {
    display: flex;
    align-items: center;
    gap: 3px;
    margin-left: -7px;
}

.city-name {
    color: #FFF;
    font-family: Montserrat;
    font-size: 20px;
    font-style: normal;
    font-weight: 600;
    line-height: normal;
}
</style>