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

    <div class="flex flex-col h-screen overflow-hidden">
      <Header></Header>
      <div
        class="flex justify-center gap-2 pt-2 bg-white w-full flex-grow select-none text-gray-900"
      >
        <slot></slot>
      </div>
      <Footer></Footer>
    </div>
    <notifications position="top center" />
  </div>
</template>
