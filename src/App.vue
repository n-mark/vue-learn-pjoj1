<script setup>
import CitySelect from './components/CitySelect.vue';
import Stat from './components/Stat.vue';

const data = {
  label: "Влажность",
  stat: "90%"
}

const user = {
  name: "Anton"
}

const handler = {
  get(target, prop, receiver) {
    // return "Vasia";
    // side-effect
    return target[prop];
  },
  set(obj, prop, value) {
    if (prop == "name") {
      console.log("Set value");
      obj[prop] = value;
      return true;
    }
  }
}

const proxy = new Proxy(user, handler);
console.log(proxy.name);
proxy.name = "asds";
console.log(proxy.name);
</script>

<template>
  <main class="main">
    <!-- {{ savedCity }} -->
    <Stat v-bind="data" />
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