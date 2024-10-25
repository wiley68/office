<script setup>
import { Head, router } from '@inertiajs/vue3'
import Footer from './Partials/Footer.vue'

defineProps({
  title: String,
})

const logout = () => {
  router.post(route('logout'))
}
</script>

<template>
  <div>
    <Head :title="title"></Head>

    <div class="flex flex-col min-h-screen">
      <Footer></Footer>
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
