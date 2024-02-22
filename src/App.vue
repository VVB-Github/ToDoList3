<script setup>
// provide - for exports
import { ref, provide } from 'vue'
// uuid - для менеджмента id
import { v4 as uuidv4 } from 'uuid';

import TodoForm from './components/TodoForm.vue';
import ToDoList from './components/ToDoList.vue';

const tasks = ref([
  {
    id: uuidv4(),
    text: 'Make ToDo app on Vue',
    done: false,
  },
  {
    id: uuidv4(),
    text: 'Go to the gym',
    done: false,
  }
]);

provide('tasks', tasks)

</script>

<template>
  <div>
    <h1>To-Do List</h1>
    <TodoForm />
    <ToDoList />
  </div>
</template>