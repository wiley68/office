<script setup>
import { useForm } from '@inertiajs/vue3'
import { ref } from 'vue'
import AppLayout from '@/Layouts/AppLayout.vue'
import StatusToggle from '@/Components/StatusToggle.vue'
import { useNotification } from '@kyvg/vue3-notification'
import Sidebar from './Partials/Sidebar.vue'

const props = defineProps({
  tasks: Array,
})

const { notify } = useNotification()
const isEditing = ref(false)
const currentTaskId = ref(null)

const form = useForm({
  name: '',
  value: '',
  status: 0,
})

const submit = () => {
  if (isEditing.value) {
    form.put(`/tasks/${currentTaskId.value}`, {
      onSuccess: () => {
        notify({
          title: 'Съобщение',
          text: 'Успешно записахте промените!',
          type: 'success',
        })
      },
    })
  } else {
    form.post('/tasks', {
      onSuccess: () => {
        notify({
          title: 'Съобщение',
          text: 'Успешно създадохте записа!',
          type: 'success',
        })
      },
    })
  }
}

const editTask = (task) => {
  form.name = task.name
  form.value = task.value
  form.status = task.status ? 1 : 0
  currentTaskId.value = task.id
  isEditing.value = true
}

const resetForm = () => {
  form.name = ''
  form.value = ''
  form.status = 0
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
    <div
      class="flex flex-col w-1/3 gap-1 p-2 mx-2 overflow-y-auto scrollbar-thin scrollbar-thumb-gray-400 scrollbar-track-gray-300"
      style="height: calc(100vh - 5rem)"
    >
      <Sidebar
        :tasks="tasks"
        :currentTaskId="currentTaskId"
        @resetForm="resetForm"
        @editTask="editTask"
        @deleteTask="deleteTask"
      ></Sidebar>
    </div>
    <div class="flex w-2/3 h-auto p-2">
      <form
        class="flex flex-grow h-full gap-1"
        @submit.prevent="submit"
      >
        <div class="flex flex-col h-full w-2/3 gap-1">
          <input
            v-model="form.name"
            placeholder="Task name"
            class="flex items-center flex-none bg-gray-50 border border-gray-200 rounded font-bold"
            :class="
              form.status ? 'line-through text-gray-400' : 'text-gray-900'
            "
          />
          <div
            v-if="form.errors.name"
            class="text-red-600"
          >
            {{ form.errors.name }}
          </div>
          <textarea
            v-model="form.value"
            placeholder="Task value"
            class="flex flex-grow bg-gray-50 border border-gray-200 rounded"
            :class="
              form.status ? 'line-through text-gray-400' : 'text-gray-900'
            "
          ></textarea>
          <div class="flex flex-none items-center h-8 mt-1 gap-2">
            <button
              type="submit"
              class="flex justify-center items-center px-2 py-1 bg-gray-100 hover:bg-gray-200 border border-gray-200 hover:border-gray-300 rounded-md"
            >
              {{ isEditing ? 'Update Task' : 'Create Task' }}
            </button>
            <button
              v-if="isEditing"
              class="flex justify-center items-center px-2 py-1 hover:bg-gray-100 hover:border-gray-200 border rounded-md"
              @click="resetForm"
            >
              Cancel
            </button>
          </div>
        </div>
        <div class="flex flex-col h-full w-1/3">
          <div
            class="flex flex-col flex-grow w-full border border-gray-200 rounded p-2"
          >
            <StatusToggle v-model="form.status" />
            <div
              v-if="form.errors.status"
              class="text-red-600"
            >
              {{ form.errors.status }}
            </div>
          </div>
          <div class="flex flex-none items-center h-8 mt-1 gap-2"></div>
        </div>
      </form>
    </div>
  </AppLayout>
</template>
