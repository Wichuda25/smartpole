<template>
    <div class="keypad-container">
        <!-- แสดงค่าที่ป้อน -->


        <!-- ปุ่มตัวเลข -->
        <div class="keypad">
            <button v-for="num in numbers" :key="num" @click="addNumber(num)">
                {{ num }}
            </button>
            <button @click="clearInput" class="clear-btn">C</button>
            <button @click="confirmInput" class="confirm-btn">OK</button>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits, computed } from "vue";

const props = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue", "confirm"]);

// ตัวเลขสำหรับแป้นพิมพ์ เรียงให้อยู่ในรูปแบบของคีย์แพดจริง ๆ
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0];

// ใช้ computed เพื่อให้แก้ไขค่า modelValue ได้
const inputValue = computed(() => props.modelValue || "");

// ฟังก์ชันเพิ่มตัวเลขที่ป้อน
const addNumber = (num) => {
    emit("update:modelValue", inputValue.value + num.toString());
};

// ล้างค่าที่ป้อน
const clearInput = () => {
    emit("update:modelValue", "");
};

// ยืนยันค่าที่ป้อน
const confirmInput = () => {
    emit("confirm");
};
</script>

<style scoped>
.keypad-container {
    text-align: center;
    margin-top: 20px;
}

.keypad-display {
    width: 100%;
    padding: 15px;
    font-size: 24px;
    text-align: center;
    margin-bottom: 10px;
    border: 2px solid #aaa;
    border-radius: 8px;
    background: #f8f8f8;
    font-weight: bold;
}

.keypad {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    justify-content: center;
}

button {
    padding: 20px;
    font-size: 24px;
    border: none;
    background: #eee;
    cursor: pointer;
    border-radius: 10px;
    font-weight: bold;
    transition: background 0.2s ease-in-out;
}

button:hover {
    background: #ccc;
}

.clear-btn {
    background: orange;
    color: white;
}

.confirm-btn {
    background: green;
    color: white;
}
</style>