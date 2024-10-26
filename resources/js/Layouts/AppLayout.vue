<script setup>
import { Head } from '@inertiajs/vue3'
import Header from './Partials/Header.vue'
import Footer from './Partials/Footer.vue'

defineProps({
  title: String,
})
</script>

<template>
  <div>
    <Head :title="title"></Head>

    <div class="flex flex-col min-h-screen">
      <Header></Header>
      <div class="flex justify-center gap-2 p-2 bg-gray-50 w-full flex-grow">
        <slot></slot>
      </div>
      <Footer></Footer>
    </div>
  </div>
</template>
