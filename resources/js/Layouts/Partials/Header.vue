<script setup>
import { Link, router } from '@inertiajs/vue3'
import ApplicationMark from '@/Components/ApplicationMark.vue'
import NavLink from '@/Components/NavLink.vue'
import { ref } from 'vue'
import StatusToggle from '@/Components/StatusToggle.vue'

const search = ref('')
const status = ref(false)

const logout = () => {
  router.post(route('logout'))
}
</script>

<template>
  <div
    class="flex justify-between bg-gray-900 w-full h-10 border-b border-neutral-200 p-1 select-none"
  >
    <div class="flex gap-2">
      <Link :href="route('dashboard')">
        <ApplicationMark class="block h-8 w-auto" />
      </Link>
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
      <div v-if="route().current('tasks.index')" class="flex gap-2">
        <div class="w-1 h-full border-r border-gray-700 mr-1"></div>
        <input
          v-model="search"
          placeholder="Search tasks..."
          class="flex items-center flex-none bg-gray-50 border border-gray-200 rounded font-medium"
        />
        <StatusToggle v-model="status" />
      </div>
    </div>
    <div class="flex gap-2">
      <NavLink
        :href="route('profile.show')"
        :active="route().current('profile.show')"
      >
        {{ $page.props.auth.user.name }}
      </NavLink>
      <form
        class="flex justify-center items-center text-neutral-200 hover:bg-gray-800 p-2 hover:rounded-md"
        @submit.prevent="logout"
      >
        <button type="submit">Log Out</button>
      </form>
    </div>
  </div>
</template>
