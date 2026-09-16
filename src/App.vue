<script setup>
import Button from './components/Button.vue';
const id = "blue";
</script>

<template>
  <header class="button">Лого</header>
  <Button>
    <template #[id]>
      +
    </template>
    <template #text>
      -
    </template>
    Сохранить
  </Button>
  <main :id>Контент</main>
</template>

<style scoped></style>