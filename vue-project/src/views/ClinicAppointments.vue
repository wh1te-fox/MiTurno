<script setup>
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
    <div class="flex items-center justify-between">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Gestión de Citas</h2>
        <p class="text-sm text-gray-500">Crea, reprograma, confirma o cancela citas médicas de la clínica.</p>
      </div>
      <button @click="$emit('new-appointment')" class="bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold px-4 py-2 rounded-xl">
        + Nueva Cita
      </button>
    </div>

    <div class="bg-white rounded-3xl shadow-xs border border-gray-100 overflow-hidden">
      <div class="overflow-x-auto">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="bg-gray-50 text-xs font-semibold text-gray-500 uppercase tracking-wider border-b border-gray-100">
              <th class="py-3.5 px-6">ID Cita</th>
              <th class="py-3.5 px-6">Paciente</th>
              <th class="py-3.5 px-6">Especialista</th>
              <th class="py-3.5 px-6">Fecha y Hora</th>
              <th class="py-3.5 px-6">Consultorio</th>
              <th class="py-3.5 px-6">Estado</th>
              <th class="py-3.5 px-6 text-right">Acciones</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-100 text-sm">
            <tr v-for="app in appointments" :key="app.id" class="hover:bg-gray-50 transition-colors">
              <td class="py-4 px-6 font-bold text-gray-900">#MT-{{ 1000 + app.id }}</td>
              <td class="py-4 px-6 font-semibold text-gray-900">{{ app.patientName }}</td>
              <td class="py-4 px-6">
                <p class="font-medium text-gray-900">{{ app.doctor }}</p>
                <p class="text-xs text-gray-500">{{ app.specialty }}</p>
              </td>
              <td class="py-4 px-6 text-gray-600">{{ app.date }}, {{ app.time }}</td>
              <td class="py-4 px-6 text-gray-600">{{ app.consultorio }}</td>
              <td class="py-4 px-6">
                <span class="text-xs font-semibold px-3 py-1 rounded-full bg-blue-100 text-blue-700">
                  {{ app.status }}
                </span>
              </td>
              <td class="py-4 px-6 text-right space-x-2">
                <button @click="$emit('view-appointment', app)" class="text-blue-600 hover:underline font-semibold text-xs">Gestionar</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
