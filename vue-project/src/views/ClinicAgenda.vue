<script setup>
import { ref } from 'vue'

const currentViewMode = ref('dia')
const selectedDate = ref('2026-10-06')

defineProps({
  appointments: {
    type: Array,
    required: true
  }
})

defineEmits(['view-appointment', 'new-appointment'])
</script>

<template>
  <div class="space-y-6 pb-12">
    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Agenda Médica</h2>
        <p class="text-sm text-gray-500">Vista detallada de la agenda por día, semana o mes.</p>
      </div>

      <div class="flex items-center space-x-3">
        <div class="bg-gray-100 p-1 rounded-xl flex">
          <button 
            @click="currentViewMode = 'dia'" 
            :class="['px-3 py-1.5 text-xs font-semibold rounded-lg transition-all', currentViewMode === 'dia' ? 'bg-white text-blue-600 shadow-xs' : 'text-gray-600']"
          >
            Día
          </button>
          <button 
            @click="currentViewMode = 'semana'" 
            :class="['px-3 py-1.5 text-xs font-semibold rounded-lg transition-all', currentViewMode === 'semana' ? 'bg-white text-blue-600 shadow-xs' : 'text-gray-600']"
          >
            Semana
          </button>
          <button 
            @click="currentViewMode = 'mes'" 
            :class="['px-3 py-1.5 text-xs font-semibold rounded-lg transition-all', currentViewMode === 'mes' ? 'bg-white text-blue-600 shadow-xs' : 'text-gray-600']"
          >
            Mes
          </button>
        </div>

        <input type="date" v-model="selectedDate" class="px-3 py-1.5 bg-white border border-gray-200 rounded-xl text-sm font-medium" />
      </div>
    </div>

    <!-- Timeline / Schedule Grid -->
    <div class="bg-white rounded-3xl shadow-xs border border-gray-100 p-6 space-y-4">
      <div class="flex items-center justify-between pb-4 border-b border-gray-100">
        <h3 class="font-bold text-gray-900">Consultorios Activos - {{ selectedDate }}</h3>
        <button @click="$emit('new-appointment')" class="bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold px-4 py-2 rounded-xl">
          + Programar Cita
        </button>
      </div>

      <div class="space-y-4">
        <div v-for="hour in ['08:00 AM', '09:00 AM', '10:00 AM', '11:00 AM', '12:00 PM', '02:00 PM', '04:00 PM']" :key="hour" class="flex items-start border-b border-gray-50 pb-4">
          <div class="w-24 text-xs font-bold text-gray-400 pt-1">{{ hour }}</div>
          <div class="flex-1 grid grid-cols-1 sm:grid-cols-3 gap-3">
            <div class="bg-blue-50 border border-blue-100 p-3 rounded-xl">
              <p class="text-xs font-bold text-blue-900">Dr. Roberto Gómez (Cardiología)</p>
              <p class="text-xs text-blue-700 mt-0.5">Paciente: María González ({{ hour }})</p>
            </div>
            <div class="bg-emerald-50 border border-emerald-100 p-3 rounded-xl">
              <p class="text-xs font-bold text-emerald-900">Dra. Sofía Martínez (Pediatría)</p>
              <p class="text-xs text-emerald-700 mt-0.5">Paciente: Lucas Pérez ({{ hour }})</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
