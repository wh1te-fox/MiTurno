<script setup>
import BaseButton from '@/components/BaseButton.vue'

defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  appointment: {
    type: Object,
    default: null
  }
})

defineEmits(['close'])
</script>

<template>
  <div v-if="isOpen && appointment" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm animate-fadeIn">
    <div class="bg-white rounded-3xl shadow-2xl w-full max-w-md overflow-hidden border border-gray-100">
      <div class="bg-gray-900 px-6 py-4 text-white flex items-center justify-between">
        <h3 class="font-bold text-lg">Detalles de la Cita</h3>
        <button @click="$emit('close')" class="text-gray-400 hover:text-white p-1">✕</button>
      </div>

      <div class="p-6 space-y-4">
        <div class="bg-emerald-50 p-4 rounded-2xl flex items-center space-x-3">
          <div class="text-3xl">🩺</div>
          <div>
            <h4 class="font-bold text-gray-900 text-lg">{{ appointment.specialty }}</h4>
            <p class="text-xs text-emerald-700 font-medium">Modalidad: {{ appointment.type || 'Presencial' }}</p>
          </div>
        </div>

        <div class="space-y-3 text-sm">
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="text-gray-500">Médico tratante</span>
            <span class="font-semibold text-gray-900">{{ appointment.doctor }}</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="text-gray-500">Fecha y Hora</span>
            <span class="font-semibold text-gray-900">{{ appointment.date }} - {{ appointment.time }}</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="text-gray-500">Clínica / Ubicación</span>
            <span class="font-semibold text-gray-900">{{ appointment.clinic }}</span>
          </div>
          <div class="flex justify-between py-2 border-b border-gray-100">
            <span class="text-gray-500">Estado</span>
            <span class="font-semibold text-emerald-600">{{ appointment.status }}</span>
          </div>
        </div>

        <div class="pt-4">
          <BaseButton buttonText="Cerrar" block @click="$emit('close')" />
        </div>
      </div>
    </div>
  </div>
</template>
