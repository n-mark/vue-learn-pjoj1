<script setup>
import IconLocation from '../icons/IconLocation.vue';
import Button from './Button.vue';
import { ref } from 'vue';

const emit = defineEmits({
    selectCity(payload) {
        console.log(`Validating payload ${payload}`);
        return payload;
    }
});

const isEditing = ref(false);

function setEditing() {
    isEditing.value = true
    console.log(`isEditing: ${isEditing.value}`);
}

function setCity() {
    emit('selectCity', "London");
    isEditing.value = false;
}

</script>

<template>
    <div v-if="isEditing" class="input-container">
        <input type="text" class="input" placeholder="Введите город">
        <Button @click="setCity()">
            Сохранить
        </Button>
    </div>
    <Button v-else @click="setEditing()">
        <IconLocation />
        Изменить город
    </Button>
</template>

<style scoped>
.input {
    background-color: var(--color-bg-card);
    box-shadow: 1px 2px 4px 0 #222831;
    border-radius: 10px;
    width: 260px;
    height: 53px;
    border: none;
    font-family: var(--font-family);
    font-weight: 400;
    font-size: 18px;
    color: #3f4958;
    padding-left: 9px;
}

.input-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 9px;
}
</style>