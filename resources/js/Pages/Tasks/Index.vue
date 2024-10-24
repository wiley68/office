<script setup>
import { useForm } from '@inertiajs/vue3'
import { defineProps, ref } from 'vue'
import AppLayout from '@/Layouts/AppLayout.vue'

// Define Props
const props = defineProps({
  tasks: Array,
})

const isEditing = ref(false)
const currentTaskId = ref(null)

// Define form for task creation
const form = useForm({
  name: '',
  value: '',
})

// Submit new task
const submit = () => {
  if (isEditing.value) {
    // Update task if in editing mode
    form.put(`/tasks/${currentTaskId.value}`, {
      onSuccess: () => resetForm(), // Reset the form after successful update
    })
  } else {
    // Create new task
    form.post('/tasks', {
      onSuccess: () => resetForm(), // Reset the form after successful creation
    })
  }
}

const editTask = (task) => {
  form.name = task.name
  form.value = task.value
  currentTaskId.value = task.id
  isEditing.value = true
}

const resetForm = () => {
  form.reset() // Reset form values
  currentTaskId.value = null
  isEditing.value = false
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
            <input
              v-model="form.name"
              placeholder="Task name"
            />
            <textarea
              v-model="form.value"
              placeholder="Task value"
            ></textarea>
            <button type="submit">
              {{ isEditing ? 'Update Task' : 'Create Task' }}
            </button>
            <button
              v-if="isEditing"
              @click="resetForm"
            >
              Cancel
            </button>
          </form>

          <!-- Task List -->
          <ul>
            <li
              v-for="task in tasks"
              :key="task.id"
            >
              <strong>{{ task.name }}</strong> - {{ task.value }}
              <button @click="editTask(task)">Edit</button>
              <button @click="deleteTask(task.id)">Delete</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </AppLayout>
</template>
