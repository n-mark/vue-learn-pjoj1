<script setup>
import { computed, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';

let savedCity = ref("Moscow");
let data = ref({
  humidity: 90,
  rain: 0,
  wind: 3
});

const dataModified = computed((prev) => {
  console.log(prev);
  return [{
    label: "Влажность",
    stat: data.value.humidity + '%'
  }, {
    label: "Осадки",
    stat: data.value.rain + '%'
  }, {
    label: "Ветер",
    stat: `${data.value.wind} м/c`
  }

  ];
});

async function getCity(city) {
  savedCity.value = city;
  data.value.humidity = 20;
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