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

onMounted(() => {
    emit("selectCity", city.value);
})

function setEditing() {
    isEditing.value = true
    console.log(`isEditing: ${isEditing.value}`);
}

function setCity() {
    emit('selectCity', city);
    isEditing.value = false;
}

</script>

<template>
    <div class="city-select">
        <div v-if="isEditing" class="input-container">
            <Input placeholder="Введите город" v-model="city" @keyup.enter="setCity()"/>
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