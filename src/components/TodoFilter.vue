<template>
  <div>
    <button @click="$emit('change', 'all')">All</button>
    <button @click="$emit('change', 'active')">Active</button>
    <button @click="$emit('change', 'completed')">Completed</button>
  </div>
</template>