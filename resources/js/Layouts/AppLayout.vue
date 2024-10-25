<script setup>
import { Head } from '@inertiajs/vue3'

defineProps({
  title: String,
})
</script>

<template>
  <div>
    <Head :title="title"></Head>

    <div class="min-h-screen bg-gray-100 dark:bg-gray-900">
      <main>
        <slot></slot>
      </main>
    </div>
  </div>
</template>
