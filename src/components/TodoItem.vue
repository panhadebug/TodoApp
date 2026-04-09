<template>
  <li>
    <span
      v-if="!isEditing"
      :style="{ textDecoration: task.done ? 'line-through' : 'none' }"
      @click="toggle"
      @dblclick="startEdit"
    >
      {{ task.text }}
    </span>

    <input
      v-else
      v-model="editText"
      @keyup.enter="saveEdit"
      @blur="saveEdit"
    />

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