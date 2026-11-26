<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  appointments: {
    type: Array,
    required: true
  }
})

defineEmits(['view-appointment', 'new-appointment'])

const statusFilter = ref('todos')

const filteredAppointments = computed(() => {
  if (statusFilter.value === 'todos') return props.appointments
  return props.appointments.filter(a => a.status.toLowerCase() === statusFilter.value.toLowerCase())
})
</script>

<template>
  <div class="space-y-8 pb-12">
    <!-- Resumen del día (KPI Cards) -->
    <section>
      <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-4">
        <div class="bg-white p-5 rounded-2xl shadow-xs border border-gray-100 flex flex-col justify-between">
          <div class="flex items-center justify-between text-gray-500 text-sm font-medium">
            <span>Citas de hoy</span>
            <span class="text-xl">📅</span>
          </div>
          <div class="mt-4">
            <h3 class="text-2xl font-black text-gray-900">24</h3>
            <p class="text-xs text-blue-600 font-medium mt-1">100% programadas</p>
          </div>
        </div>

        <div class="bg-white p-5 rounded-2xl shadow-xs border border-gray-100 flex flex-col justify-between">
          <div class="flex items-center justify-between text-gray-500 text-sm font-medium">
            <span>Atendidos</span>
            <span class="text-xl">✅</span>
          </div>
          <div class="mt-4">
            <h3 class="text-2xl font-black text-emerald-600">8</h3>
            <p class="text-xs text-emerald-600 font-medium mt-1">En curso o finalizadas</p>
          </div>
        </div>

        <div class="bg-white p-5 rounded-2xl shadow-xs border border-gray-100 flex flex-col justify-between">
          <div class="flex items-center justify-between text-gray-500 text-sm font-medium">
            <span>Pendientes</span>
            <span class="text-xl">⏳</span>
          </div>
          <div class="mt-4">
            <h3 class="text-2xl font-black text-amber-500">12</h3>
            <p class="text-xs text-amber-600 font-medium mt-1">Próximas horas</p>
          </div>
        </div>

        <div class="bg-white p-5 rounded-2xl shadow-xs border border-gray-100 flex flex-col justify-between">
          <div class="flex items-center justify-between text-gray-500 text-sm font-medium">
            <span>Canceladas</span>
            <span class="text-xl">❌</span>
          </div>
          <div class="mt-4">
            <h3 class="text-2xl font-black text-red-500">2</h3>
            <p class="text-xs text-red-600 font-medium mt-1">Reprogramables</p>
          </div>
        </div>

        <div class="bg-white p-5 rounded-2xl shadow-xs border border-gray-100 flex flex-col justify-between">
          <div class="flex items-center justify-between text-gray-500 text-sm font-medium">
            <span>En espera</span>
            <span class="text-xl">🪑</span>
          </div>
          <div class="mt-4">
            <h3 class="text-2xl font-black text-blue-600">4</h3>
            <p class="text-xs text-blue-600 font-medium mt-1">En sala de espera</p>
          </div>
        </div>

        <div class="bg-white p-5 rounded-2xl shadow-xs border border-gray-100 flex flex-col justify-between">
          <div class="flex items-center justify-between text-gray-500 text-sm font-medium">
            <span>Médicos</span>
            <span class="text-xl">🩺</span>
          </div>
          <div class="mt-4">
            <h3 class="text-2xl font-black text-gray-900">10 / 12</h3>
            <p class="text-xs text-emerald-600 font-medium mt-1">Disponibles hoy</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Agenda del día -->
    <section class="bg-white rounded-3xl shadow-xs border border-gray-100 overflow-hidden">
      <div class="p-6 border-b border-gray-100 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
        <div>
          <h2 class="text-xl font-bold text-gray-900">Agenda de Hoy</h2>
          <p class="text-sm text-gray-500">Listado cronológico de consultas médicas para el día de hoy.</p>
        </div>

        <div class="flex items-center space-x-3">
          <select v-model="statusFilter" class="px-3 py-2 bg-gray-50 border border-gray-200 rounded-xl text-sm font-medium text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500">
            <option value="todos">Todos los estados</option>
            <option value="Confirmada">Confirmada</option>
            <option value="En espera">En espera</option>
            <option value="En consulta">En consulta</option>
            <option value="Completada">Completada</option>
            <option value="Cancelada">Cancelada</option>
          </select>
          <button @click="$emit('new-appointment')" class="bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold px-4 py-2 rounded-xl transition-all">
            + Nueva Cita
          </button>
        </div>
      </div>

      <!-- Table -->
      <div class="overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="bg-gray-50 text-xs font-semibold text-gray-500 uppercase tracking-wider border-b border-gray-100">
              <th class="py-3.5 px-6">Hora</th>
              <th class="py-3.5 px-6">Paciente</th>
              <th class="py-3.5 px-6">Médico / Especialidad</th>
              <th class="py-3.5 px-6">Consultorio</th>
              <th class="py-3.5 px-6">Tipo</th>
              <th class="py-3.5 px-6">Estado</th>
              <th class="py-3.5 px-6 text-right">Acciones</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-100 text-sm">
            <tr v-for="app in filteredAppointments" :key="app.id" class="hover:bg-gray-50/70 transition-colors">
              <td class="py-4 px-6 font-bold text-gray-900">{{ app.time }}</td>
              <td class="py-4 px-6 font-semibold text-gray-900">{{ app.patientName }}</td>
              <td class="py-4 px-6">
                <p class="font-medium text-gray-900">{{ app.doctor }}</p>
                <p class="text-xs text-gray-500">{{ app.specialty }}</p>
              </td>
              <td class="py-4 px-6 text-gray-600 font-medium">{{ app.consultorio }}</td>
              <td class="py-4 px-6">
                <span class="text-xs bg-gray-100 text-gray-700 px-2.5 py-1 rounded-full font-medium">{{ app.type }}</span>
              </td>
              <td class="py-4 px-6">
                <span :class="[
                  'text-xs font-semibold px-3 py-1 rounded-full inline-block',
                  app.status === 'Confirmada' ? 'bg-blue-100 text-blue-700' :
                  app.status === 'En espera' ? 'bg-amber-100 text-amber-700' :
                  app.status === 'En consulta' ? 'bg-purple-100 text-purple-700' :
                  app.status === 'Completada' ? 'bg-emerald-100 text-emerald-700' : 'bg-red-100 text-red-700'
                ]">
                  {{ app.status }}
                </span>
              </td>
              <td class="py-4 px-6 text-right space-x-2">
                <button @click="$emit('view-appointment', app)" class="text-blue-600 hover:text-blue-800 font-semibold text-xs bg-blue-50 px-3 py-1.5 rounded-lg transition-colors">
                  Ver
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>
