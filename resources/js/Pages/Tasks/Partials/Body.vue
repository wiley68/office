<script setup>
import StatusToggle from '@/Components/StatusToggle.vue'

const props = defineProps({
  form: Object,
  isEditing: Boolean,
})

defineEmits(['submit', 'resetForm'])
</script>

<template>
  <div class="flex w-2/3 h-auto p-2">
    <form
      class="flex flex-grow h-full gap-1"
      @submit.prevent="$emit('submit')"
    >
      <div class="flex flex-col h-full w-2/3 gap-1">
        <input
          v-model="form.name"
          placeholder="Task name"
          class="flex items-center flex-none bg-gray-50 border border-gray-200 rounded font-bold"
          :class="form.status ? 'line-through text-gray-400' : 'text-gray-900'"
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
          :class="form.status ? 'line-through text-gray-400' : 'text-gray-900'"
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
            @click="$emit('resetForm')"
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
</template>
