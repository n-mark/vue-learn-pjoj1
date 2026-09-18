<script setup>
import { computed, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';

let savedCity = ref("Moscow");
let data = ref({
  humidity: 90,

});

// нет реактивной зависимости, закешируется на момент расчета
const date = computed(() => {
  return new Date();
});

const dataModified = computed((prev) => {
  console.log(prev);
  return {
    label: "Влажность",
    stat: data.value.humidity + '%'
  }
});


async function getCity(city) {
  savedCity.value = city;
  data.value.humidity = 20;
}
</script>

<template>
  <main class="main">
    {{ date }}
    <div id="city">{{ savedCity }}</div>
    <Stat v-bind="dataModified" />
    <Stat label="Осадки" stat="0%" />
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