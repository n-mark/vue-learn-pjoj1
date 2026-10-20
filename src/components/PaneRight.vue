<script setup>
import CitySelect from "./CitySelect.vue";
import Stat from "./Stat.vue";
import Error from "./Error.vue";
import ConditionCard from "./ConditionCard.vue";
import { computed } from "vue";

const { error, data, activeIndex } = defineProps({ 
  error: Object,
  data: Object,
  activeIndex: Number,
});

const emit = defineEmits(["select-index", "select-city"]);

const errorMap = new Map([[1006, "Указанный город не найден"]]);

const errorDisplay = computed(() => {
  return errorMap.get(error?.error?.code);
});

const dataModified = computed(() => {
  if (!data) {
    return [];
  }

  return [
    {
      label: "Влажность",
      stat: data.forecast.forecastday[activeIndex].hour[0].humidity + " %",
    },
    {
      label: "Вероятность дождя",
      stat: data.forecast.forecastday[activeIndex].hour[0].chance_of_rain + " %",
    },
    {
      label: "Ветер",
      stat: data.forecast.forecastday[activeIndex].hour[0].wind_kph + " км/ч",
    },
  ];
});

const forecast = computed(() => {
  if (!data) {
    return [];
  }

  return [
    {
      weatherCode: data.forecast.forecastday[0].day.condition.code,
      date: data.forecast.forecastday[0].date,
      temperature: data.forecast.forecastday[0].day.maxtemp_c,
    },
    {
      weatherCode: data.forecast.forecastday[1].day.condition.code,
      date: data.forecast.forecastday[1].date,
      temperature: data.forecast.forecastday[1].day.maxtemp_c,
    },
    {
      weatherCode: data.forecast.forecastday[2].day.condition.code,
      date: data.forecast.forecastday[2].date,
      temperature: data.forecast.forecastday[2].day.maxtemp_c,
    },
  ];
});
</script>

<template>
  <Error v-if="error" :error="errorDisplay" />
  <div v-if="data" class="stat-data">
    <div class="stat-list">
      <Stat v-for="item in dataModified" v-bind="item" :key="item.label" />
    </div>
    <div class="day-card-list">
      <ConditionCard
        v-for="(value, idx) in forecast"
        v-bind="value"
        :is-active="activeIndex == idx"
        :key="value.date"
        @click="() => emit('select-index', idx)"
      />
    </div>
  </div>
  <CitySelect />
</template>

<style scoped>
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
