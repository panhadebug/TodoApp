<template>
  <div>
    <h1>TODO APP</h1>

    <!-- Input -->
    <input
      v-model="newTask"
      placeholder="Add a new task"
      @keyup.enter="addTask"
    />
    <button @click="addTask">Add Task</button>
    <button @click="clearCompleted">Clear Completed</button>
    <p>{{ remainingTasks }} tasks remaining</p>
    <!-- Filter -->
    <div>
      <button @click="filter = 'all'">All</button>
      <button @click="filter = 'active'">Active</button>
      <button @click="filter = 'completed'">Completed</button>
    </div>

    <!-- Task List -->
    <ul>
      <li v-for="(task, index) in filteredTasks" :key="index">
        <span
          :style="{
            textDecoration: task.done ? 'line-through' : 'none',
            cursor: 'pointer',
          }"
          @click="toggleTask(index)"
        >
          {{ task.text }}
        </span>

        <button @click="removeTask(index)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, watch, computed } from "vue";

// state
const filter = ref("all");
const newTask = ref("");
const tasks = ref([]);
const remainingTasks = computed(() => {
  return tasks.value.filter((task) => !task.done).length;
});
const clearCompleted = () => {
  tasks.value = tasks.value.filter((task) => !task.done);
};

const filteredTasks = computed(() => {
  if (filter.value === "active") {
    return tasks.value.filter((task) => !task.done);
  } else if (filter.value === "completed") {
    return tasks.value.filter((task) => task.done);
  }
  return tasks.value;
});

// load saved tasks
const savedTasks = localStorage.getItem("tasks");
if (savedTasks) {
  tasks.value = JSON.parse(savedTasks);
}

// add task
const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({
      text: newTask.value,
      done: false,
    });
    newTask.value = "";
  }
};

// remove task
const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

// toggle complete
const toggleTask = (index) => {
  tasks.value[index].done = !tasks.value[index].done;
};

// save tasks to localStorage
watch(
  tasks,
  (newTasks) => {
    localStorage.setItem("tasks", JSON.stringify(newTasks));
  },
  { deep: true },
);
</script>

<style>
body {
  font-family: Arial;
  background: #525050;
}

div {
  max-width: 400px;
  margin: 40px auto;
  background: rgb(0, 76, 41);
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
