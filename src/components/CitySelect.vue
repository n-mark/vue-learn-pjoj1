<script setup>
import IconLocation from '../icons/IconLocation.vue';
import Input from './Input.vue';
import Button from './Button.vue';
import { ref } from 'vue';

const emit = defineEmits({
    selectCity(payload) {
        console.log(`Validating payload ${payload}`);
        return payload;
    }
});

let city = ref("Москва");
const isEditing = ref(false);

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
        <div v-show="isEditing" class="input-container">
            <!-- two-way binding -->
            <Input placeholder="Введите город" :value="city" @update:value="updateCity" />
            <Button @click="setCity()">
                Сохранить
            </Button>
        </div>
        <Button v-show="!isEditing" @click="setEditing()">
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