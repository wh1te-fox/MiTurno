<script setup>
import BaseButton from '@/components/BaseButton.vue'

defineProps({
  appointment: {
    type: Object,
    required: true
  }
})

defineEmits(['view-details', 'reschedule', 'cancel'])
</script>

<template>
  <div class="bg-gradient-to-br from-emerald-600 via-teal-700 to-teal-900 text-white rounded-3xl p-6 sm:p-8 shadow-xl relative overflow-hidden">
    <!-- Decorative elements -->
    <div class="absolute -right-16 -top-16 w-64 h-64 bg-white/10 rounded-full blur-2xl pointer-events-none"></div>

    <div class="flex flex-col md:flex-row md:items-center justify-between gap-6 relative z-10">
      <div class="space-y-4">
        <div class="inline-flex items-center space-x-2 px-3 py-1 bg-white/20 backdrop-blur-md rounded-full text-xs font-semibold tracking-wide uppercase">
          <span class="w-2 h-2 rounded-full bg-emerald-300 animate-ping"></span>
          <span>Próxima Cita Confirmada</span>
        </div>

        <div>
          <h3 class="text-2xl sm:text-3xl font-black tracking-tight">{{ appointment.specialty }}</h3>
          <p class="text-emerald-100 text-base mt-1">Con <span class="font-semibold text-white">{{ appointment.doctor }}</span></p>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 pt-2 text-sm text-emerald-100">
          <div class="flex items-center space-x-2">
            <span>📅</span>
            <span>{{ appointment.date }}</span>
          </div>
          <div class="flex items-center space-x-2">
            <span>⏰</span>
            <span>{{ appointment.time }}</span>
          </div>
          <div class="flex items-center space-x-2">
            <span>📍</span>
            <span>{{ appointment.clinic }}</span>
          </div>
        </div>
      </div>

      <!-- Action buttons -->
      <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-3 pt-4 md:pt-0 border-t md:border-t-0 border-white/20">
        <BaseButton buttonText="Ver detalles" variant="white" @click="$emit('view-details', appointment)" />
        <BaseButton buttonText="Reprogramar" variant="outline" class="border-white text-white hover:bg-white/10" @click="$emit('reschedule', appointment)" />
        <button 
          @click="$emit('cancel', appointment)"
          class="text-xs font-medium text-emerald-200 hover:text-white underline text-center sm:text-left py-1 px-2"
        >
          Cancelar cita
        </button>
      </div>
    </div>
  </div>
</template>
