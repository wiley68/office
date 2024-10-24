<script setup>
import { useForm } from '@inertiajs/vue3'
import { defineProps, ref } from 'vue'
import AppLayout from '@/Layouts/AppLayout.vue'

// Define Props
const props = defineProps({
  tasks: Array,
})

// Define form for task creation
const form = useForm({
  name: '',
  value: '',
})

// Submit new task
const submit = () => {
  form.post('/tasks')
}

// Delete a task
const deleteTask = (id) => {
  if (confirm('Are you sure you want to delete this task?')) {
    form.delete(`/tasks/${id}`)
  }
}
</script>

<template>
  <AppLayout title="Dashboard">
    <template #header>
      <h2
        class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight"
      >
        Dashboard
      </h2>
    </template>

    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div
          class="bg-white dark:bg-gray-800 overflow-hidden shadow-xl sm:rounded-lg"
        >
          <form @submit.prevent="submit">
            <input v-model="form.name" placeholder="Task name" />
            <textarea v-model="form.value" placeholder="Task value"></textarea>
            <button type="submit">Create Task</button>
          </form>

          <!-- Task List -->
          <ul>
            <li v-for="task in tasks" :key="task.id">
              {{ task.name }} - {{ task.value }}
              <button @click="deleteTask(task.id)">Delete</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </AppLayout>
</template>
