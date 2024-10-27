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
    resetForm()
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
        class="flex items-center w-full border border-gray-200 p-2 cursor-pointer select-none hover:bg-gray-200 hover:border-gray-300"
        :class="
          currentTaskId === task.id
            ? 'bg-gray-200 border-gray-300'
            : 'bg-gray-50'
        "
      >
        <div
          class="flex items-center flex-grow"
          @click="currentTaskId === task.id ? resetForm() : editTask(task)"
        >
          <strong>{{ task.name }}</strong>
        </div>
        <div class="flex-none">
          <button
            class="px-2 py-1 rounded-md"
            @click="deleteTask(task.id)"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="w-4 h-4 text-red-400 hover:text-red-600"
            >
              <path
                d="M19,4H15.5L14.5,3H9.5L8.5,4H5V6H19M6,19A2,2 0 0,0 8,21H16A2,2 0 0,0 18,19V7H6V19Z"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>
    <div class="flex flex-1 h-auto p-2">
      <form
        class="flex flex-col flex-grow h-full gap-1"
        @submit.prevent="submit"
      >
        <input
          v-model="form.name"
          placeholder="Task name"
          class="flex items-center flex-none bg-gray-50 border border-gray-200 rounded"
        />
        <textarea
          v-model="form.value"
          placeholder="Task value"
          class="flex flex-grow"
        ></textarea>
        <div class="flex flex-none">
          <button type="submit">
            {{ isEditing ? 'Update Task' : 'Create Task' }}
          </button>
          <button
            v-if="isEditing"
            @click="resetForm"
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  </AppLayout>
</template>
