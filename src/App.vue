<script setup>
import { ref } from "vue";
const name = ref('John Doe');
const status = ref(true);
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const link = ref('test.com');

const toggleStatus = () => {
  status.value = !status.value
}

const newTask = ref('');


</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status">User is active</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks</h3>
  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>
  <a :href="link">Link</a>

  <button @click="toggleStatus">Change status</button>
</template>

<style scoped></style>