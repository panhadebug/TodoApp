<template>
  <div>
    <input 
      v-model="task" 
      placeholder="Add a new task"
      @keyup.enter="submitTask"
    />
    <button @click="submitTask">Add</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["add"]);

const task = ref("");

const submitTask = () => {
  if (task.value.trim() !== "") {
    emit("add", task.value);
    task.value = "";
  }
};
</script>