<script setup>
const pastAppointments = [
  { id: 101, specialty: 'Dermatología', doctor: 'Dr. Alejandro Silva', date: '12 Ene 2026', time: '03:30 PM', clinic: 'Clínica Central', status: 'Completada', type: 'Presencial' },
  { id: 102, specialty: 'Medicina General', doctor: 'Dr. Juan Pérez', date: '05 Nov 2025', time: '09:00 AM', clinic: 'Sanatorio Norte', status: 'Completada', type: 'Virtual' },
  { id: 103, specialty: 'Pediatría', doctor: 'Dra. Sofía Martínez', date: '18 Ago 2025', time: '11:00 AM', clinic: 'Centro Pediátrico', status: 'Completada', type: 'Presencial' }
]
</script>

<template>
  <div class="space-y-6 pb-12">
    <div>
      <h2 class="text-2xl font-bold text-gray-900">Historial Médico</h2>
      <p class="text-sm text-gray-500">Consulta tus citas pasadas y antecedentes médicos registrados.</p>
    </div>

    <div class="space-y-4">
      <div v-for="app in pastAppointments" :key="app.id" class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 flex items-center justify-between">
        <div class="flex items-center space-x-4">
          <div class="w-12 h-12 rounded-2xl bg-gray-100 text-gray-600 font-bold flex items-center justify-center text-xl">
            📁
          </div>
          <div>
            <h4 class="font-bold text-gray-900">{{ app.specialty }} - {{ app.doctor }}</h4>
            <p class="text-xs text-gray-500">{{ app.date }} • {{ app.clinic }} • Modalidad: {{ app.type }}</p>
          </div>
        </div>
        <span class="text-xs font-semibold text-gray-700 bg-gray-100 px-3 py-1 rounded-full">
          {{ app.status }}
        </span>
      </div>
    </div>
  </div>
</template>
