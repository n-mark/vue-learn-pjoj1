<script setup>
import { computed, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';

const API_ENDPOINT = "https://api.weatherapi.com/v1";

let savedCity = ref("Moscow");
let data = ref();

const dataModified = computed((prev) => {
  if (!data.value) {
    return [];
  }
  console.log(prev);
  return [{
    label: "Влажность",
    stat: data.value.current.humidity + '%'
  }, {
    label: "Облачность",
    stat: data.value.current.cloud + '%'
  }, {
    label: "Ветер",
    stat: `${data.value.current.wind_kph} км/ч`
  }

  ];
});

async function getCity(city) {
  const params = new URLSearchParams({
    q: city.value,
    lang: "ru",
    key: "0620fb37fdbb4607bf9101920262809",
    days: 3
  })
  const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`);
  data.value = await res.json();
}
</script>

<template>
  <main class="main">
    <div id="city">{{ savedCity }}</div>
    <Stat v-for="info in dataModified" v-bind="info" :key="info.label" />
    <CitySelect @select-city="getCity" />
  </main>
</template>

<style scoped>
.main {
  background: var(--color-bg-main);
  padding: 60px 50px;
  border-radius: 25px;
}
</style>