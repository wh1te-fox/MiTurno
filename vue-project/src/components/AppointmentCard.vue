<script setup>
import BaseButton from '@/components/BaseButton.vue'

defineProps({
  appointment: {
    type: Object,
    required: true
  },
  isPast: {
    type: Boolean,
    default: false
  }
})

defineEmits(['view-details', 'reschedule'])
</script>

<template>
  <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 hover:shadow-md transition-shadow flex flex-col sm:flex-row sm:items-center justify-between gap-4">
    <div class="flex items-start space-x-4">
      <div class="w-12 h-12 rounded-2xl bg-emerald-50 text-emerald-600 font-bold flex items-center justify-center text-xl flex-shrink-0">
        {{ isPast ? '📁' : '🩺' }}
      </div>
      <div class="space-y-1">
        <div class="flex items-center space-x-2">
          <h4 class="font-bold text-gray-900 text-base">{{ appointment.specialty }}</h4>
          <span :class="[
            'text-[10px] font-semibold px-2 py-0.5 rounded-full',
            isPast ? 'bg-gray-100 text-gray-600' : 'bg-emerald-100 text-emerald-800'
          ]">
            {{ appointment.status }}
          </span>
        </div>
        <p class="text-sm text-gray-700 font-medium">{{ appointment.doctor }}</p>
        <p class="text-xs text-gray-500">{{ appointment.date }} a las {{ appointment.time }} • {{ appointment.clinic }}</p>
      </div>
    </div>

    <div class="flex items-center space-x-3 pt-2 sm:pt-0 border-t sm:border-t-0 border-gray-100">
      <BaseButton buttonText="Ver detalles" size="sm" variant="outline" @click="$emit('view-details', appointment)" />
      <BaseButton v-if="!isPast" buttonText="Reprogramar" size="sm" variant="secondary" @click="$emit('reschedule', appointment)" />
    </div>
  </div>
</template>
