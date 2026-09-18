<script setup>
import IconLocation from '../icons/IconLocation.vue';
import Input from './Input.vue';
import Button from './Button.vue';
import { onBeforeMount, onMounted, onUnmounted, onUpdated, onWatcherCleanup, ref, watch, watchEffect } from 'vue';

const emit = defineEmits({
    selectCity(payload) {
        console.log(`Validating payload ${payload}`);
        return payload;
    }
});

let city = ref("Москва");
const isEditing = ref(false);

// // следить за изменением в заданном рефе (1й аргумент)
// watch(city, (newValue, oldValue) => {
//     console.log(`new city: ${newValue}`);
//     console.log(`old city: ${oldValue}`);
// },
//     // вызвать сразу после монтирования
//     // { immediate: true }

//     // вызвать один раз
//     // { once: true }
// );


// // когда любой из тех которые внутри будут меняться
// watchEffect(() => {
//     console.log(`watcheffect city: ${city.value}`);
//     console.log(`watcheffect isediting: ${isEditing.value}`);
// });

// watch(city, () => {
//     console.log(city.value);
//     // при следующей смене значения происходит cleanup
//     // отменить таймер
//     // или запрос к back'y и т.д.
//     onWatcherCleanup(() => {
//         console.log("cleanup");
//     });
// });

// const unwatch = watchEffect(() => {});
// unwatch();


function setEditing() {
    isEditing.value = true
    console.log(`isEditing: ${isEditing.value}`);
}

function setCity() {
    emit('selectCity', "London");
    isEditing.value = false;
}

function updateCity(newCity) {
    console.log(newCity);
    city.value = newCity;
}

</script>

<template>
    {{ city }}
    <div class="city-select">
        <div v-if="isEditing" class="input-container">
            <!-- можно иметь несколько v-model -->
            <!-- <Input placeholder="Введите город" v-model="city" v-model:additional="city" /> -->
            <Input placeholder="Введите город" v-model="city" />
            <Button @click="setCity()">
                Сохранить
            </Button>
        </div>
        <Button v-if="!isEditing" @click="setEditing()">
            <IconLocation />
            Изменить город
        </Button>
    </div>
</template>

<style scoped>
.input-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
}

.city-select {
    width: 420px;
}
</style>