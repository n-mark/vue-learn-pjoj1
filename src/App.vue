<!-- composition api -->
<script setup>
const id = "blue";
</script>

<!-- <script>
// option api
export default {
  data() {
    return {
      id: "blue"
    }
  },
  methods: {

  }
}
</script> -->

<template>
  <header :class="`color-${id}`">Лого</header>
  <main :id>Контент</main>
</template>

<style scoped></style>