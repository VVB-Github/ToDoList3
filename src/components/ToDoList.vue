<script setup>
// для импорта inject
import { inject, ref, computed } from 'vue'

//переменная которая отвечает за таск который мы сейчас реда-м
const editingTaskId = ref(null)
const editingText = ref('')
// import task-list
const tasks = inject('tasks')

//функция удаления
const deleteTask = (taskId) => {
    /* Находим индекс задачи в массиве
    findIndex - 
        проходит по каждому элементу массива и выполняет предоставленную
        функцию до тех пор, пока эта функция не вернёт true
    task => task.id === taskId - 
        Эта конкретная функция используется для проверки каждого элемента массива на
        соответствие заданному условию. В данном контексте, она проверяет, равен ли
        id каждого объекта задачи (task) заданному taskId.
    */
    const index = tasks.value.findIndex(task => task.id === taskId);
    /* findIndex - вернет -1 если не нашел элемент */
    if (index !== -1) {
        /* Удаляем задачу из массива 

        .splice(index, 1):
            Это вызов метода splice() на массиве. Метод splice() изменяет содержимое
            массива, позволяя добавлять или удалять элементы. В данном случае метод
            используется для удаления элемента из массива.
        index:
            Это аргумент, который указывает начальную позицию в массиве для удаления
            элементов. В контексте предыдущего обсуждения, index является индексом
            элемента, который нужно удалить, найденным с помощью метода findIndex.
        1:
            Это второй аргумент метода splice(), который указывает количество
            элементов для удаления, начиная с позиции, заданной первым аргументом.
            В данном случае, указание 1 означает, что будет удален только один
            элемент — тот, который находится на позиции index.
        */
        tasks.value.splice(index, 1);
    }
}
//функция смены статуса выполнения
const changeStatus = (taskId) => {
    const index = tasks.value.findIndex(task => task.id === taskId)
    if (index !== -1) {
        tasks.value[index].done = !tasks.value[index].done
    }
}
//function for counting done tasks
const tasksDoneFunc = computed(() => {
    let count = 0
    for (let task of tasks.value) {
        if (task.done) {
            count += 1
        }
    }
    return count
})
//function for counting tasks
const tasksCount = computed(() => {
    let count = 0
    for (let task of tasks.value) {
        count += 1
    }
    return count
})
//ф-я для запуска едита задачи
function startEdit(taskId, text) {
    editingTaskId.value = taskId
    editingText.value = text
}
//ф-я закрытия едита
function cancelEdit() {
    editingTaskId.value = null;
    editingText.value = '';
}
//ф-я для сохранения изменённого задания
function saveEdit(taskId) {
    const index = tasks.value.findIndex(task => task.id === taskId)
    if (index !== -1) {
        tasks.value[index].text = editingText.value
    }
    editingTaskId.value = null;
    editingText.value = '';
}

</script>

<template>
    <div style="width: 400px; overflow: auto; max-width: 400px;">
        <h2>Tasks done {{ tasksDoneFunc }} of {{ tasksCount }}</h2>
        <ul class="todo_list list-group">
            <li v-for="task in tasks" :key="task.id"
                class="list-group-item d-flex justify-content-between align-items-center">
                <input @click="changeStatus(task.id)" type="checkbox" class="form-check-input me-1">
                <!-- код для редактирования задачи -->
                <div v-if="editingTaskId === task.id">
                    <input type="text" v-model="editingText" class="form-control"
                        style="display: inline-block; width: auto;">
                    <button @click="saveEdit(task.id)" class="btn btn-success btn-sm me-2">Save</button>
                    <button @click="cancelEdit" class="btn btn-secondary btn-sm">Cancel</button>
                </div>
                <!-- код для показа задачи -->
                <div v-else>
                    {{ task.text }}
                    <button @click="startEdit(task.id, task.text)" class="btn btn-primary btn-sm me-2">Edit</button>
                    <button @click="deleteTask(task.id)" class="btn btn-danger btn-sm">Delete</button>
                </div>
            </li>
        </ul>
    </div>
</template>

<style>
.todo_list {
    list-style-type: none;
}
</style>