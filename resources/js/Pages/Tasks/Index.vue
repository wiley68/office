<script setup>
import { useForm } from '@inertiajs/vue3'
import { ref } from 'vue'
import AppLayout from '@/Layouts/AppLayout.vue'
import { useNotification } from '@kyvg/vue3-notification'
import Sidebar from './Partials/Sidebar.vue'
import Body from './Partials/Body.vue'

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
    <Sidebar
      :tasks="tasks"
      :currentTaskId="currentTaskId"
      @resetForm="resetForm"
      @editTask="editTask"
      @deleteTask="deleteTask"
    ></Sidebar>
    <Body
      :form="form"
      :isEditing="isEditing"
      @submit.prevent="submit"
      @resetForm="resetForm"
    ></Body>
  </AppLayout>
</template>
