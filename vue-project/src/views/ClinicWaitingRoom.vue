<script setup>
import { ref } from 'vue'

const waitingPatients = ref([
  { id: 1, name: 'María González', arrivalTime: '09:40 AM', appTime: '10:00 AM', doctor: 'Dr. Roberto Gómez', specialty: 'Cardiología', consultorio: 'Cons. 402', waitTimeMinutes: 35, status: 'En espera' },
  { id: 2, name: 'Juan Pérez', arrivalTime: '10:10 AM', appTime: '10:30 AM', doctor: 'Dra. Sofía Martínez', specialty: 'Pediatría', consultorio: 'Cons. 201', waitTimeMinutes: 15, status: 'Llegó' },
  { id: 3, name: 'Lucía Torres', arrivalTime: '09:15 AM', appTime: '09:30 AM', doctor: 'Dr. Alejandro Silva', specialty: 'Dermatología', consultorio: 'Cons. 105', waitTimeMinutes: 45, status: 'En consulta' },
  { id: 4, name: 'Esteban Morales', arrivalTime: '10:25 AM', appTime: '11:00 AM', doctor: 'Dr. Mario Vargas', specialty: 'Oftalmología', consultorio: 'Cons. 308', waitTimeMinutes: 8, status: 'En espera' }
])

const statuses = ['Llegó', 'En espera', 'En consulta', 'Atendido']

function updateStatus(patient, newStatus) {
  patient.status = newStatus
}
</script>

<template>
  <div class="space-y-6 pb-12">
    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Sala de Espera Activa</h2>
        <p class="text-sm text-gray-500">Supervisa la llegada, tiempo de espera y flujo de pacientes en recepción.</p>
      </div>

      <div class="flex items-center space-x-3 text-xs">
        <span class="inline-flex items-center space-x-1.5 px-3 py-1.5 bg-red-50 text-red-700 rounded-lg font-semibold border border-red-100">
          <span class="w-2 h-2 rounded-full bg-red-500 animate-pulse"></span>
          <span>> 30 min (Alerta)</span>
        </span>
        <span class="inline-flex items-center space-x-1.5 px-3 py-1.5 bg-emerald-50 text-emerald-700 rounded-lg font-semibold border border-emerald-100">
          <span class="w-2 h-2 rounded-full bg-emerald-500"></span>
          <span>Tiempo óptimo</span>
        </span>
      </div>
    </div>

    <!-- Live Waiting Grid / Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <div 
        v-for="patient in waitingPatients" 
        :key="patient.id"
        :class="[
          'bg-white rounded-3xl p-6 shadow-xs border transition-all flex flex-col justify-between space-y-4',
          patient.waitTimeMinutes > 30 ? 'border-red-200 ring-2 ring-red-500/20' : 'border-gray-100'
        ]"
      >
        <div class="space-y-3">
          <div class="flex items-center justify-between">
            <span :class="[
              'text-xs font-bold px-2.5 py-1 rounded-full',
              patient.waitTimeMinutes > 30 ? 'bg-red-100 text-red-700' : 'bg-blue-100 text-blue-700'
            ]">
              ⏱️ Espera: {{ patient.waitTimeMinutes }} min
            </span>
            <span class="text-xs font-semibold text-gray-500">{{ patient.consultorio }}</span>
          </div>

          <div>
            <h3 class="font-extrabold text-gray-900 text-lg">{{ patient.name }}</h3>
            <p class="text-xs text-gray-500">Cita: {{ patient.appTime }} • Llegada: {{ patient.arrivalTime }}</p>
          </div>

          <div class="bg-gray-50 p-3 rounded-2xl text-xs space-y-1">
            <p class="font-bold text-gray-900">{{ patient.doctor }}</p>
            <p class="text-gray-500">{{ patient.specialty }}</p>
          </div>
        </div>

        <!-- Status Change Flow -->
        <div class="pt-4 border-t border-gray-100">
          <label class="block text-[11px] font-bold text-gray-500 uppercase tracking-wider mb-2">Estado actual</label>
          <div class="grid grid-cols-2 gap-1.5">
            <button 
              v-for="st in statuses" 
              :key="st"
              @click="updateStatus(patient, st)"
              :class="[
                'py-1.5 px-2 rounded-lg text-xs font-semibold transition-all',
                patient.status === st 
                  ? 'bg-blue-600 text-white shadow-xs' 
                  : 'bg-gray-100 text-gray-600 hover:bg-gray-200'
              ]"
            >
              {{ st }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
