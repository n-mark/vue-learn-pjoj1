<script setup>
import { ref, inject } from "vue";
import IconLocation from "../icons/IconLocation.vue";
import Button from "./Button.vue";
import Input from "./Input.vue";

let isEdited = ref(false);

const city = inject("city");
const inputValue = ref(city);

function select() {
  isEdited.value = false;
  city.value = inputValue.value;
}

function edit() {
  isEdited.value = true;
}
</script>

<template>
  <div class="city-select">
    <div v-if="isEdited" class="city-input">
      <Input
        v-model="inputValue"
        placeholder="Введите город"
        @keyup.enter="select()"
      />
      <Button @click="select()">Сохранить</Button>
    </div>
    <Button v-if="!isEdited" @click="edit()">
      <IconLocation />
      Изменить город
    </Button>
  </div>
</template>

<style scoped>
.city-input {
  display: flex;
  gap: 12px;
}

.city-select {
  width: 420px;
}
</style>
