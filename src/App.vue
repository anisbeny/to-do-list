<template>
    <h1>to-do-list</h1>
    <form action="" @submit.prevent="addTask">
        <fieldset role="group">
            <input type="text" v-model="newTask" placeholder="Enter your task" />
            <button :disabled="newTask.length === 0">add</button>
        </fieldset>
    </form>
    <div v-if="tasks.length === 0">Empty list :(</div>
    <div v-else>
        <ul>
            <li v-for="task in sortedTasks()" :key="task.date" :class="{ completed: task.done }">
                <input type="checkbox" v-model="task.done" />
                {{ task.title }}
            </li>
        </ul>
        <label>
            <input type="checkbox" v-model="hideCompleted" />
            Hide completed tasks
        </label>
    </div>
</template>
<script setup>
import { h, ref } from 'vue';
const tasks = ref([]);
const newTask = ref('');
const hideCompleted = ref(false);
const addTask = () => {
    tasks.value.push({
        title: newTask.value,
        done: false,
        date: Date.now(),
    });
    newTask.value = '';
};
const sortedTasks = () => {
    const sortedTasks = tasks.value.toSorted((a, b) => (a.done > b.done ? 1 : -1));
    return hideCompleted.value ? sortedTasks.filter((task) => !task.done) : sortedTasks;
};
</script>

<style>
li {
    list-style: none !important;
}
.completed {
    text-decoration: line-through;
    opacity: 0.5;
}
</style>
