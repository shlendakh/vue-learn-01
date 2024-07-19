<script setup>
import { ref, onMounted } from "vue";
const name = ref('John Doe');
const status = ref(true);
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const link = ref('test.com');

const toggleStatus = () => {
  status.value = !status.value
}

const newTask = ref('');

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deletTask = (index) => {
  tasks.value.splice(index, 1);
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();

    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log(`Error while fetching JSON Todos`);
  }
});

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
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deletTask(index)">X</button>
    </li>
  </ul>
  <a :href="link">Link</a>

  <button @click="toggleStatus">Change status</button>
</template>

<style scoped></style>