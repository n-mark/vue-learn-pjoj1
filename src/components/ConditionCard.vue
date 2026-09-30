<script setup>
import { computed } from "vue";
const props = defineProps({
    icon: [Object, Function],
    dayOfWeek: String,
    temperature: Number,
    unitOfMeasure: String,
    type: String
});


const iconColor = computed(() => {
    return props.type === 'current' ? 'black' : 'white';
})

</script>

<template>
    <div class="card" :class="type">
        <component :is="icon" :color="iconColor" />
        <div class="day">{{ dayOfWeek }}</div>
        <div class="info">{{ temperature }} {{ unitOfMeasure }}</div>
    </div>
</template>

<style scoped>
/* Общие стили карточки — вынести в отдельный класс */
.card {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    width: 104px;
    height: 168px;
    border-radius: 10px;
    padding: 15px;
    transition: background-color 0.3s ease;
}

/* Стили по типу */
.current {
    background-color: aliceblue;
}

.forecast {
    background-color: #272e37;
}

.forecast:hover {
    box-shadow: 1px 2px 4px 0 #222831;
    background: #3a434f;
}

/* Вложенные элементы внутри .current */
.current .info {
    font-family: var(--font-family);
    font-weight: 700;
    font-size: 20px;
    text-align: center;
    color: #000;
}

.current .day {
    font-family: var(--font-family);
    font-weight: 400;
    font-size: 20px;
    text-align: center;
    color: #000;
}

/* Вложенные элементы внутри .forecast */
.forecast .info {
    font-family: var(--font-family);
    font-weight: 700;
    font-size: 20px;
    text-align: center;
    color: #fff;
}

.forecast .day {
    font-family: var(--font-family);
    font-weight: 400;
    font-size: 20px;
    text-align: center;
    color: #fff;
}
</style>