<script setup>
import { Head, Link } from '@inertiajs/vue3'
import ApplicationMark from '@/Components/ApplicationMark.vue'
import NavLink from '@/Components/NavLink.vue'

defineProps({
  title: String,
})
</script>

<template>
  <div>
    <Head :title="title"></Head>

    <div class="flex flex-col min-h-screen">
      <div
        class="flex justify-between bg-gray-900 w-full h-10 border-b border-neutral-200 p-1"
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
        </div>
        <div>111</div>
      </div>
      <div class="flex justify-center bg-neutral-50 w-full flex-grow">
        <slot></slot>
      </div>
      <div
        class="flex items-center bg-neutral-100 w-full h-8 border-t border-neutral-200"
      >
        111
      </div>
    </div>
  </div>
</template>
