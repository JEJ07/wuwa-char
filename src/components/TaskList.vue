<script setup>
import { onMounted, ref } from "vue";
const tasks = ref([]);
const newTask = ref("");
const taskErrorMessage = ref("");

const isEditing = ref(null);
const editedTask = ref("");
const editErrorMessage = ref("");

const addTask = () => {
    if (newTask.value.trim() === "") {
        taskErrorMessage.value = "Task can't be empty!";
        return;
    }

    tasks.value.push(newTask.value);
    newTask.value = "";
    taskErrorMessage.value = "";
};

const editTask = (index) => {
    isEditing.value = index;
    editedTask.value = tasks.value[index];
    editErrorMessage.value = "";
};

const saveTask = (index) => {
    if (editedTask.value.trim() === "") {
        editErrorMessage.value = "Edited task can't be empty!";
        return;
    }

    tasks.value[index] = editedTask.value;
    isEditing.value = null;
    editedTask.value = "";
    editErrorMessage.value = "";
};

const cancelEdit = () => {
    isEditing.value = null;
    editedTask.value = "";
    editErrorMessage.value = "";
};

const deleteTask = (index) => {
    tasks.value.splice(index, 1);
};
// GET TODOS API
onMounted(async () => {
    try {
        const response = await fetch("https://jsonplaceholder.typicode.com/todos");
        const data = await response.json();
        tasks.value = data.map((task) => task.title);
    } catch (error) {
        console.log("error fetching api task");
    }
});
</script>
<template>
    <div class="min-h-screen bg-gray-100 py-8 px-4">
        <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">All Tasks</h1>

        <div class="container mx-auto max-w-3xl bg-white p-6 rounded-lg shadow-md">
            <form @submit.prevent="addTask" class="mb-6">
                <h2 class="text-xl font-semibold mb-2 text-gray-700">Add a Task</h2>
                <label for="addnewTask" class="block text-sm font-medium text-gray-600 mb-1">Task Name</label>
                <input type="text" id="addnewTask" v-model="newTask"
                    class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-400" />
                <button type="submit"
                    class="mt-3 bg-blue-500 hover:bg-blue-600 text-white font-semibold py-2 px-4 rounded">
                    Submit
                </button>
                <p v-if="taskErrorMessage" class="text-red-500 mt-2 text-sm">
                    {{ taskErrorMessage }}
                </p>
            </form>

            <h3 class="text-lg font-semibold mb-4 text-gray-700">Tasks</h3>
            <ul class="space-y-4">
                <li v-for="(task, index) in tasks" :key="index"
                    class="bg-gray-50 p-4 rounded border flex justify-between items-start">
                    <div class="w-full">
                        <div v-if="isEditing === index" class="space-y-2">
                            <input type="text" v-model="editedTask"
                                class="w-full px-3 py-2 border border-gray-300 rounded focus:outline-none focus:ring-2 focus:ring-blue-400" />
                            <div class="flex gap-2 mt-2">
                                <button @click="saveTask(index)"
                                    class="bg-green-500 hover:bg-green-600 text-white px-3 py-1 rounded">
                                    Save
                                </button>
                                <button @click="cancelEdit"
                                    class="bg-gray-400 hover:bg-gray-500 text-white px-3 py-1 rounded">
                                    Cancel
                                </button>
                            </div>
                            <p v-if="editErrorMessage" class="text-red-500 text-sm">
                                {{ editErrorMessage }}
                            </p>
                        </div>

                        <div v-else class="flex justify-between items-center">
                            <span class="text-gray-800">{{ task }}</span>
                            <div class="flex gap-2">
                                <button @click="editTask(index)"
                                    class="bg-yellow-400 hover:bg-yellow-500 text-white px-3 py-1 rounded">
                                    Edit
                                </button>
                                <button @click="deleteTask(index)"
                                    class="bg-red-500 hover:bg-red-600 text-white px-3 py-1 rounded">
                                    Delete
                                </button>
                            </div>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>
