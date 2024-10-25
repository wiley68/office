<script setup>
import { useForm, Link, router } from '@inertiajs/vue3'
import { ref } from 'vue'
import AppLayout from '@/Layouts/AppLayout.vue'
import NavLink from '@/Components/NavLink.vue'

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

const logout = () => {
  router.post(route('logout'))
}
</script>

<template>
  <AppLayout title="Dashboard">
    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div
          class="bg-white dark:bg-gray-800 overflow-hidden shadow-xl sm:rounded-lg"
        >
          <NavLink
            :href="route('dashboard')"
            :active="route().current('dashboard')"
          >
            Dashboard
          </NavLink>
          <NavLink
            :href="route('tasks.index')"
            :active="route().current('tasks.index')"
          >
            Tasks
          </NavLink>
          <Link :href="route('profile.show')"> Profile </Link>
          <Link
            v-if="$page.props.jetstream.hasApiFeatures"
            :href="route('api-tokens.index')"
          >
            API Tokens
          </Link>
          <form @submit.prevent="logout">
            <button type="submit">Log Out</button>
          </form>
          <form @submit.prevent="submit">
            <input v-model="form.name" placeholder="Task name" />
            <textarea v-model="form.value" placeholder="Task value"></textarea>
            <button type="submit">
              {{ isEditing ? 'Update Task' : 'Create Task' }}
            </button>
            <button v-if="isEditing" @click="resetForm">Cancel</button>
          </form>

          <ul>
            <li v-for="task in tasks" :key="task.id">
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
