<script setup>
import AppointmentCard from '@/components/AppointmentCard.vue'
import BaseButton from '@/components/BaseButton.vue'

defineProps({
  appointments: {
    type: Array,
    required: true
  }
})

defineEmits(['view-details', 'reschedule', 'open-booking'])
</script>

<template>
  <div class="space-y-6 pb-12">
    <div class="flex items-center justify-between">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Mis Citas Médicas</h2>
        <p class="text-sm text-gray-500">Gestiona y consulta todas tus citas programadas.</p>
      </div>
      <BaseButton buttonText="Agendar nueva cita" @click="$emit('open-booking')" />
    </div>

    <div class="space-y-4">
      <AppointmentCard 
        v-for="app in appointments" 
        :key="app.id" 
        :appointment="app"
        @view-details="$emit('view-details', $event)"
        @reschedule="$emit('reschedule', $event)"
      />
    </div>
  </div>
</template>
