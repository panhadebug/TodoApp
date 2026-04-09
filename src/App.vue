<template>
  <div :class="{ dark: darkMode }">
    <h1>TODO APP</h1>

    <button @click="darkMode = !darkMode">
      Toggle {{ darkMode ? 'Light' : 'Dark' }}
    </button>

    <TodoInput @add="addTask" />

    <TodoFilter @change="filter = $event" />

    <p>{{ remainingTasks }} tasks left</p>

    <ul>
      <TodoItem
        v-for="(task, index) in filteredTasks"
        :key="index"
        :task="task"
        @toggle="toggleTask(index)"
        @remove="removeTask(index)"
        @edit="editTask(index, $event)"
      />
    </ul>

    <button @click="clearCompleted">Clear Completed</button>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";

import TodoInput from "./components/TodoInput.vue";
import TodoItem from "./components/TodoItem.vue";
import TodoFilter from "./components/TodoFilter.vue";

const tasks = ref([]);
const filter = ref("all");
const darkMode = ref(false);

// load
const saved = localStorage.getItem("tasks");
if (saved) tasks.value = JSON.parse(saved);

// add
const addTask = (text) => {
  tasks.value.push({ text, done: false });
};

// remove
const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

// toggle
const toggleTask = (index) => {
  tasks.value[index].done = !tasks.value[index].done;
};

// edit
const editTask = (index, newText) => {
  tasks.value[index].text = newText;
};

// filter
const filteredTasks = computed(() => {
  if (filter.value === "active") return tasks.value.filter(t => !t.done);
  if (filter.value === "completed") return tasks.value.filter(t => t.done);
  return tasks.value;
});

// count
const remainingTasks = computed(() => {
  return tasks.value.filter(t => !t.done).length;
});

// clear
const clearCompleted = () => {
  tasks.value = tasks.value.filter(t => !t.done);
};

// save
watch(tasks, (val) => {
  localStorage.setItem("tasks", JSON.stringify(val));
}, { deep: true });
</script>

<style>
body {
  font-family: Arial;
  background: #f5f5f5;
}

.dark {
  background: #222;
  color: white;
}

div {
  max-width: 400px;
  margin: 40px auto;
  background: white;
  padding: 20px;
  border-radius: 10px;
}

.dark div {
  background: #333;
}
</style>

<style>
body {
  font-family: Arial;
  background: #525050;
}

div {
  max-width: 400px;
  margin: 40px auto;
  background: rgb(65, 64, 103);
  padding: 20px;
  border-radius: 10px;
}

h1 {
  text-align: center;
}

input {
  padding: 8px;
  width: 70%;
}

button {
  padding: 6px 10px;
  margin: 5px;
  cursor: pointer;
}

li {
  display: flex;
  justify-content: space-between;
  margin: 10px 0;
}
</style>
