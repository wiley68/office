<script setup>
import { useForm } from '@inertiajs/vue3'
import { ref } from 'vue'
import AppLayout from '@/Layouts/AppLayout.vue'

const props = defineProps({
  tasks: Array,
})

const isEditing = ref(false)
const currentTaskId = ref(null)

const form = useForm({
  name: '',
  value: '',
})

const submit = () => {
  if (isEditing.value) {
    form.put(`/tasks/${currentTaskId.value}`, {
      onSuccess: () => resetForm(),
    })
  } else {
    form.post('/tasks', {
      onSuccess: () => resetForm(),
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
  form.reset()
  currentTaskId.value = null
  isEditing.value = false
}

const deleteTask = (id) => {
  if (confirm('Are you sure you want to delete this task?')) {
    form.delete(`/tasks/${id}`)
  }
}
</script>

<template>
  <AppLayout title="Dashboard">
    <div class="flex flex-col flex-1 h-auto gap-1 p-2">
      <div
        v-for="task in tasks"
        :key="task.id"
        class="flex items-center w-full border border-gray-200 p-2 cursor-pointer select-none"
        :class="
          currentTaskId === task.id
            ? 'bg-gray-200 border-gray-300'
            : 'bg-gray-50'
        "
        @click="editTask(task)"
      >
        <div class="flex items-center flex-grow">
          <strong>{{ task.name }}</strong> - {{ task.value }}
        </div>
        <div class="flex-none">
          <button @click="deleteTask(task.id)">Delete</button>
        </div>
      </div>
    </div>
    <div class="flex flex-1 h-auto p-2">
      <form
        class="flex flex-col flex-grow h-full"
        @submit.prevent="submit"
      >
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
    </div>
  </AppLayout>
</template>
