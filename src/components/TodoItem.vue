<template>
  <li>
  <span
    :style="{ 
      textDecoration: task.done ? 'line-through' : 'none',
      cursor: 'pointer',
      transition: '0.2s'
    }"
    @click="toggle"
    @dblclick="startEdit"
  >
    {{ task.text }}
  </span>

  <button @click="$emit('remove')">❌</button>
</li>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps(["task"]);
const emit = defineEmits(["toggle", "remove", "edit"]);

const isEditing = ref(false);
const editText = ref(props.task.text);

const toggle = () => emit("toggle");

const startEdit = () => {
  isEditing.value = true;
};

const saveEdit = () => {
  emit("edit", editText.value);
  isEditing.value = false;
};
</script>