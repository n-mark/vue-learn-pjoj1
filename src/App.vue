<script setup>
import { computed, ref } from "vue";
import CitySelect from "./components/CitySelect.vue";
import Stat from "./components/Stat.vue";
import Error from "./components/Error.vue";
import ConditionCard from "./components/ConditionCard.vue";
const API_ENDPOINT = "https://api.weatherapi.com/v1";

const errorMap = new Map([[1006, "Указанный город не найден"]]);

let data = ref();
let error = ref();

const errorDisplay = computed(() => {
  return errorMap.get(error.value?.error?.code);
});

const dataModified = computed(() => {
  return [
    {
      label: "Влажность",
      stat: data.value.current.humidity + " %",
    },
    {
      label: "Облачность",
      stat: data.value.current.cloud + " %",
    },
    {
      label: "Ветер",
      stat: data.value.current.wind_kph + " км/ч",
    },
  ];
});

const forecast = computed(() => {
  return [
    {
      weatherCode: data.value.forecast.forecastday[0].day.condition.code,
      date: data.value.forecast.forecastday[0].date,
      temperature: data.value.forecast.forecastday[0].day.maxtemp_c
    },
    {
      weatherCode: data.value.forecast.forecastday[1].day.condition.code,
      date: data.value.forecast.forecastday[1].date,
      temperature: data.value.forecast.forecastday[1].day.maxtemp_c
    },
    {
      weatherCode: data.value.forecast.forecastday[2].day.condition.code,
      date: data.value.forecast.forecastday[2].date,
      temperature: data.value.forecast.forecastday[2].day.maxtemp_c
    },
    {
      weatherCode: data.value.forecast.forecastday[3].day.condition.code,
      date: data.value.forecast.forecastday[3].date,
      temperature: data.value.forecast.forecastday[3].day.maxtemp_c
    }
  ]
})

async function getCity(city) {
  const params = new URLSearchParams({
    q: city,
    lang: "ru",
    key: "0620fb37fdbb4607bf9101920262809",
    days: 4
  })
  const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`);

  if (res.status != 200) {
    error.value = await res.json();
    data.value = null;
    return;
  }
  error.value = null;
  data.value = await res.json();
}

</script>

<template>
  <main class="main">
    <Error :error="errorDisplay" />
    <div v-if="data" class="stat-data">
      <div class="stat-list">
        <Stat v-for="item in dataModified" v-bind="item" :key="item.label" />
      </div>
      <div class="day-card-list">
        <ConditionCard v-for="value in forecast" v-bind="value" :key="value.date" />
      </div>
    </div>
    <CitySelect @select-city="getCity" />
  </main>
</template>

<style scoped>
.main {
  background: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
}

.day-card-list {
  display: flex;
  gap: 1px;
}

.stat-data {
  display: flex;
  flex-direction: column;
  gap: 80px;
  margin-bottom: 70px;
}

.stat-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
</style>