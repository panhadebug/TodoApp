<template>
  <div :class="{ dark: darkMode }">
    <h1>TODO APP</h1>

    <button @click="darkMode = !darkMode">
  {{ darkMode ? '☀ Light Mode' : '🌙 Dark Mode' }}
</button>

    <TodoInput @add="addTask" />

    <TodoFilter @change="filter = $event" />

    <p>{{ remainingTasks }} tasks left</p>

    <transition-group name="fade" tag="ul">
      <TodoItem
        v-for="(task, index) in filteredTasks"
        :key="task.text + index"
        :task="task"
        @toggle="toggleTask(index)"
        @remove="removeTask(index)"
        @edit="editTask(index, $event)"
      />
    </transition-group>

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
  if (filter.value === "active") return tasks.value.filter((t) => !t.done);
  if (filter.value === "completed") return tasks.value.filter((t) => t.done);
  return tasks.value;
});

// count
const remainingTasks = computed(() => {
  return tasks.value.filter((t) => !t.done).length;
});

// clear
const clearCompleted = () => {
  tasks.value = tasks.value.filter((t) => !t.done);
};

// save
watch(
  tasks,
  (val) => {
    localStorage.setItem("tasks", JSON.stringify(val));
  },
  { deep: true },
);
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #667eea, #764ba2);
  margin: 0;
}

/* App container */
div {
  max-width: 420px;
  margin: 50px auto;
  background: white;
  padding: 25px;
  border-radius: 15px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.2);
  transition: 0.3s;
}

/* Dark mode */
.dark {
  background: #1e1e2f;
  color: white;
}

.dark div {
  background: #2a2a40;
}

/* Title */
h1 {
  text-align: center;
  margin-bottom: 20px;
}

/* Input */
input {
  padding: 10px;
  width: 65%;
  border-radius: 8px;
  border: 1px solid #ccc;
  outline: none;
}

/* Buttons */
button {
  padding: 8px 12px;
  margin: 5px;
  border: none;
  border-radius: 8px;
  background: #667eea;
  color: white;
  cursor: pointer;
  transition: 0.2s;
}

button:hover {
  transform: scale(1.05);
  background: #5a67d8;
}

/* Task item */
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f3f4f6;
  padding: 10px;
  border-radius: 10px;
  margin: 8px 0;
  transition: 0.2s;
}

li:hover {
  transform: translateX(5px);
}

/* Dark task */
.dark li {
  background: #3a3a55;
}
span {
  display: inline-block;
}

span:active {
  transform: scale(0.95);
}
</style>
