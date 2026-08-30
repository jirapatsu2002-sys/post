<script setup>
import { ref } from 'vue'

const newTodo = ref('')
const count = ref(0)

function increment() {
  count.value += 1  // ใน script ใช้ .value
}
</script>

<template>
    <!-- template แกะ .value ให้อัตโนมัติ -->
  <p>{{ count }}</p>
  
  <button class="todo-btn" @click="increment">เพิ่มจำนวน</button>
</template>

