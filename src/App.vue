<script setup>
import { computed, ref } from "vue";
import CitySelect from "./components/CitySelect.vue";
import Stat from "./components/Stat.vue";
import Error from "./components/Error.vue";
import IconSun from "./icons/IconSun.vue";
import IconCloud from "./icons/IconCloud.vue";
import IconRain from "./icons/IconRain.vue";
import ConditionCard from "./components/ConditionCard.vue";
const API_ENDPOINT = "https://api.weatherapi.com/v1";

const errorMap = new Map([[1006, "Указанный город не найден"]]);
const conditionsMap = new Map([[1000, IconSun], [1009, IconRain]], [1006, IconCloud]);

const CELCIUS = "°C";

let data = ref();
let error = ref();

const errorDisplay = computed(() => {
  return errorMap.get(error.value?.error?.code);
});

const forecast = computed(() => {
  if (!data.value) {
    return [];
  }

  console.log(data.value);

  return [
    // {
    //   icon: conditionsMap.get(data.value.current.condition.code) ?? IconSun,
    //   dayOfWeek: getShortWeekdayRu(data.value.current.last_updated_epoch),
    //   temperature: data.value.current.temp_c,
    //   unitOfMeasure: CELCIUS,
    //   type: "current"
    // },
    {
      icon: conditionsMap.get(data.value.forecast.forecastday[0].day.condition.code) ?? IconSun,
      dayOfWeek: getShortWeekdayRu(data.value.forecast.forecastday[0].date_epoch),
      temperature: data.value.forecast.forecastday[0].day.maxtemp_c,
      unitOfMeasure: CELCIUS,
      type: "current"
    },
    {
      icon: conditionsMap.get(data.value.forecast.forecastday[1].day.condition.code) ?? IconSun,
      dayOfWeek: getShortWeekdayRu(data.value.forecast.forecastday[1].date_epoch),
      temperature: data.value.forecast.forecastday[1].day.maxtemp_c,
      unitOfMeasure: CELCIUS,
      type: "forecast"
    },
    {
      icon: conditionsMap.get(data.value.forecast.forecastday[2].day.condition.code) ?? IconSun,
      dayOfWeek: getShortWeekdayRu(data.value.forecast.forecastday[2].date_epoch),
      temperature: data.value.forecast.forecastday[2].day.maxtemp_c,
      unitOfMeasure: CELCIUS,
      type: "forecast"
    }
  ]
})

const dataModified = computed(() => {
  if (!data.value) {
    return [];
  }
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

async function getCity(city) {
  const params = new URLSearchParams({
    q: city,
    lang: "ru",
    key: "0620fb37fdbb4607bf9101920262809",
    days: 3
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


function getShortWeekdayRu(input) {
  let date;

  if (typeof input === 'number') {
    // Если меньше 1e12 — считаем, что это секунды
    date = new Date(input < 1e12 ? input * 1000 : input);
  } else if (typeof input === 'string') {
    // Нормализуем "YYYY-MM-DD hh:mm" → "YYYY-MM-DDThh:mm"
    // и "YYYY-MM-DD" → "YYYY-MM-DDT00:00:00Z"
    let normalized = input.trim().replace(' ', 'T');
    if (/^\d{4}-\d{2}-\d{2}$/.test(normalized)) {
      normalized += 'T00:00:00Z';
    } else if (!/[Zz]|[+-]\d{2}:?\d{2}$/.test(normalized)) {
      // Нет таймзоны — считаем UTC
      normalized += 'Z';
    }
    date = new Date(normalized);
  } else {
    return '';
  }

  if (isNaN(date.getTime())) return '';

  const short = ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'];
  return short[date.getUTCDay()];
}


</script>

<template>
  <main class="main">
    <Error :error="errorDisplay" />
    <Stat v-for="item in dataModified" v-bind="item" :key="item.label" />
    <div class="forecast-area">
      <ConditionCard v-for="value in forecast" v-bind="value" :key="value.dayOfWeek" />
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

.forecast-area {
  display: flex;
  justify-content: space-around;
  margin-bottom: 71px;
  margin-top: 79px;
  gap: 1px;
}
</style>