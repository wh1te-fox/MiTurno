<script setup>
import NextAppointmentCard from '@/components/NextAppointmentCard.vue'
import AppointmentCard from '@/components/AppointmentCard.vue'
import BaseButton from '@/components/BaseButton.vue'

defineProps({
  appointments: {
    type: Array,
    required: true
  }
})

defineEmits(['view-details', 'reschedule', 'cancel', 'open-booking', 'navigate'])
</script>

<template>
  <div class="space-y-8 pb-12">
    <!-- Next Appointment Section -->
    <section>
      <div class="flex items-center justify-between mb-4">
        <h2 class="text-lg font-bold text-gray-900">Próxima cita</h2>
        <button @click="$emit('navigate', 'appointments')" class="text-xs font-semibold text-emerald-600 hover:underline">Ver todas</button>
      </div>

      <NextAppointmentCard 
        v-if="appointments.length > 0" 
        :appointment="appointments[0]" 
        @view-details="$emit('view-details', $event)"
        @reschedule="$emit('reschedule', $event)"
        @cancel="$emit('cancel', $event)"
      />
      <div v-else class="bg-white p-8 rounded-3xl border border-gray-100 text-center space-y-3">
        <p class="text-gray-500">No tienes citas próximas programadas.</p>
        <BaseButton buttonText="Agendar nueva cita" @click="$emit('open-booking')" />
      </div>
    </section>

    <!-- Upcoming Appointments List -->
    <section>
      <div class="flex items-center justify-between mb-4">
        <h2 class="text-lg font-bold text-gray-900">Mis próximas citas</h2>
        <BaseButton buttonText="+ Agendar cita" size="sm" @click="$emit('open-booking')" />
      </div>

      <div class="space-y-4">
        <AppointmentCard 
          v-for="app in appointments.slice(1, 3)" 
          :key="app.id" 
          :appointment="app"
          @view-details="$emit('view-details', $event)"
          @reschedule="$emit('reschedule', $event)"
        />
        <p v-if="appointments.length <= 1" class="text-sm text-gray-500 bg-white p-6 rounded-2xl border border-gray-100 text-center">
          No hay más citas próximas registradas.
        </p>
      </div>
    </section>

    <!-- Past Appointments Preview -->
    <section>
      <div class="flex items-center justify-between mb-4">
        <h2 class="text-lg font-bold text-gray-900">Citas anteriores</h2>
        <button @click="$emit('navigate', 'history')" class="text-xs font-semibold text-emerald-600 hover:underline">Ver historial completo</button>
      </div>

      <div class="space-y-3">
        <div class="bg-white p-4 rounded-2xl border border-gray-100 flex items-center justify-between text-sm">
          <div class="flex items-center space-x-3">
            <span class="text-2xl">📋</span>
            <div>
              <p class="font-bold text-gray-900">Dermatología - Dr. Alejandro Silva</p>
              <p class="text-xs text-gray-500">12 Ene 2026 • Clínica Central</p>
            </div>
          </div>
          <span class="text-xs font-semibold text-gray-600 bg-gray-100 px-2.5 py-1 rounded-full">Completada</span>
        </div>
      </div>
    </section>
  </div>
</template>
