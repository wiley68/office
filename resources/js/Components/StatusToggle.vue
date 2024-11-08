<template>
  <div class="flex items-center space-x-4">
    <span :class="statusClass">{{ statusText }}</span>
    <button
      @click.prevent="toggleStatus"
      class="px-4 py-2 text-white font-semibold rounded-md transition"
      :class="buttonClass"
    >
      Toggle Status
    </button>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { defineEmits } from 'vue'

const props = defineProps({
  modelValue: {
    type: Number,
    default: 0,
    validator: (value) => [0, 1].includes(value),
  },
})

const emit = defineEmits(['update:modelValue'])

const status = ref(props.modelValue)

// Watch for changes in the prop to keep `status` in sync
watch(
  () => props.modelValue,
  (newValue) => {
    status.value = newValue
  }
)

const statusText = computed(() => (status.value === 1 ? 'Active' : 'Inactive'))
const statusClass = computed(() =>
  status.value === 1
    ? 'text-green-500 font-bold'
    : 'text-gray-500 font-semibold'
)
const buttonClass = computed(() =>
  status.value === 1
    ? 'bg-red-500 hover:bg-red-600'
    : 'bg-green-500 hover:bg-green-600'
)

const toggleStatus = () => {
  status.value = status.value === 1 ? 0 : 1
  emit('update:modelValue', status.value) // Emit the new value to update the parent component's v-model
}
</script>
