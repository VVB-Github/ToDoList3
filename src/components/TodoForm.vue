<script setup>
// inject - for imports
import { ref, inject } from 'vue'
// uuidv4 - id manegment
import { v4 as uuidv4 } from 'uuid';

const tasks = inject('tasks')
const new_task = ref('')

//ф-я добавления задачи
const addTask = () => {
    const newTask = {
        id: uuidv4(),
        text: new_task.value,
        done: false
    };
    tasks.value.push(newTask)
    new_task.value = ''
}

</script>

<template>
    <form class="add_form my-3">
        <div>
            <label for="newTask" class="form-label">What needs to be done?</label>
            <input type="text" class="form-control" id="newTask" v-model="new_task" placeholder="Enter a new task" />
        </div>
        <button @click.prevent="addTask" class="btn btn-primary">Add</button>
    </form>
</template>

<style>
.add_form {
    display: flex;
    flex-direction: column;
}

.btn-primary {
    background-color: #6f42c1 !important; /* Фиолетовый цвет фона */
    border-color: #5a33a2 !important; /* Фиолетовый цвет границы */
}

.btn-primary:hover, .btn-primary:focus {
    background-color: #5a33a2 !important; /* Темно-фиолетовый цвет фона при наведении или фокусе */
    border-color: #482885 !important; /* Темно-фиолетовый цвет границы при наведении или фокусе */
}

</style>