<script setup>
import { computed } from 'vue'

const props = defineProps({
  buttonText: {
    type: String,
    default: ''
  },
  type: {
    type: String,
    default: 'button'
  },
  variant: {
    type: String,
    default: 'primary',
    validator: (val) => ['primary', 'secondary', 'outline', 'white', 'dark'].includes(val)
  },
  size: {
    type: String,
    default: 'md',
    validator: (val) => ['sm', 'md', 'lg'].includes(val)
  },
  disabled: {
    type: Boolean,
    default: false
  },
  block: {
    type: Boolean,
    default: false
  }
})

const variantClasses = computed(() => {
  switch (props.variant) {
    case 'secondary':
      return 'bg-emerald-100 text-emerald-800 hover:bg-emerald-200 active:bg-emerald-300 focus:ring-emerald-400'
    case 'outline':
      return 'border-2 border-emerald-600 text-emerald-600 hover:bg-emerald-50 active:bg-emerald-100 focus:ring-emerald-500'
    case 'white':
      return 'bg-white text-gray-900 hover:bg-gray-100 active:bg-gray-200 shadow-md focus:ring-emerald-400'
    case 'dark':
      return 'bg-gray-900 text-white hover:bg-gray-800 active:bg-black focus:ring-gray-700'
    case 'primary':
    default:
      return 'bg-emerald-600 text-white hover:bg-emerald-700 active:bg-emerald-800 shadow-sm focus:ring-emerald-500'
  }
})

const sizeClasses = computed(() => {
  switch (props.size) {
    case 'sm':
      return 'px-3 py-1.5 text-xs font-medium'
    case 'lg':
      return 'px-6 py-3.5 text-base font-semibold'
    case 'md':
    default:
      return 'px-4 py-2.5 text-sm font-medium'
  }
})
</script>

<template>
  <button 
    :type="type"
    :disabled="disabled"
    :class="[
      'inline-flex items-center justify-center rounded-xl transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed cursor-pointer',
      variantClasses,
      sizeClasses,
      block ? 'w-full' : ''
    ]"
  >
    <slot>{{ buttonText }}</slot>
  </button>
</template>
