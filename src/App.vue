<script setup>
import { computed, reactive, ref } from 'vue';
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';

let savedCity = ref("Moscow");
let data = ref({
  humidity: 90,

});

const dataModified = computed((prev) => {
  console.log(prev);
  return {
    label: "Влажность",
    stat: data.value.humidity + '%'
  }
});

const arr = ref(['Антон', 'Вася', 'Марина']);
const obj = ref({
  name: "Антон",
  age: 18
});

async function getCity(city) {
  savedCity.value = city;
  data.value.humidity = 20;
}
</script>

<template>
  <main class="main">
    <ul>
      <!-- <li v-for="item in arr" :key="item"> -->
      <!-- С индексом -->
      <li v-for="(item, index) in arr" :key="item">
        {{ index }} : {{ item }}
      </li>
    </ul>
    <ul>
      <li v-for="(value, key, idx) in obj", :key="key">
        {{ idx }} : {{ value }}, {{ key }}
      </li>
    </ul>
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