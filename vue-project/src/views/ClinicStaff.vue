<script setup>
import { ref } from 'vue'

const doctors = ref([
  { id: 1, name: 'Dr. Roberto Gómez', specialty: 'Cardiología', status: 'En consulta', consultorio: 'Consultorio 402', schedule: '08:00 - 14:00', todayAppointments: 8, avatar: 'RG' },
  { id: 2, name: 'Dra. Sofía Martínez', specialty: 'Pediatría', status: 'Disponible', consultorio: 'Consultorio 201', schedule: '09:00 - 15:00', todayAppointments: 6, avatar: 'SM' },
  { id: 3, name: 'Dr. Alejandro Silva', specialty: 'Dermatología', status: 'Disponible', consultorio: 'Consultorio 105', schedule: '10:00 - 16:00', todayAppointments: 5, avatar: 'AS' },
  { id: 4, name: 'Dr. Mario Vargas', specialty: 'Oftalmología', status: 'En pausa', consultorio: 'Consultorio 308', schedule: '12:00 - 18:00', todayAppointments: 4, avatar: 'MV' },
  { id: 5, name: 'Dra. María Benítez', specialty: 'Medicina General', status: 'Disponible', consultorio: 'Consultorio 101', schedule: '08:00 - 13:00', todayAppointments: 7, avatar: 'MB' },
  { id: 6, name: 'Dra. Carmen Ríos', specialty: 'Odontología', status: 'En consulta', consultorio: 'Consultorio 204', schedule: '14:00 - 20:00', todayAppointments: 5, avatar: 'CR' }
])

const selectedDoctor = ref(null)

function viewDoctorAgenda(doc) {
  selectedDoctor.value = doc
}
</script>

<template>
  <div class="space-y-6 pb-12">
    <div class="flex items-center justify-between">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Médicos y Personal Clínico</h2>
        <p class="text-sm text-gray-500">Supervisión de disponibilidad, consultorios asignados y agendas individuales.</p>
      </div>
    </div>

    <!-- Doctors Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div 
        v-for="doc in doctors" 
        :key="doc.id" 
        class="bg-white rounded-3xl p-6 shadow-xs border border-gray-100 flex flex-col justify-between space-y-4"
      >
        <div class="flex items-start justify-between">
          <div class="flex items-center space-x-4">
            <div class="w-12 h-12 rounded-2xl bg-blue-100 text-blue-700 font-bold flex items-center justify-center text-base">
              {{ doc.avatar }}
            </div>
            <div>
              <h3 class="font-bold text-gray-900 text-base">{{ doc.name }}</h3>
              <p class="text-xs text-blue-600 font-semibold">{{ doc.specialty }}</p>
            </div>
          </div>
          <span :class="[
            'text-[10px] font-bold px-2.5 py-1 rounded-full uppercase',
            doc.status === 'Disponible' ? 'bg-emerald-100 text-emerald-700' :
            doc.status === 'En consulta' ? 'bg-purple-100 text-purple-700' : 'bg-amber-100 text-amber-700'
          ]">
            {{ doc.status }}
          </span>
        </div>

        <div class="bg-gray-50 rounded-2xl p-4 text-xs space-y-2 text-gray-600">
          <div class="flex justify-between">
            <span>Consultorio:</span>
            <span class="font-bold text-gray-900">{{ doc.consultorio }}</span>
          </div>
          <div class="flex justify-between">
            <span>Horario:</span>
            <span class="font-bold text-gray-900">{{ doc.schedule }}</span>
          </div>
          <div class="flex justify-between">
            <span>Citas de hoy:</span>
            <span class="font-bold text-blue-600">{{ doc.todayAppointments }} pacientes</span>
          </div>
        </div>

        <button 
          @click="viewDoctorAgenda(doc)"
          class="w-full bg-blue-50 hover:bg-blue-100 text-blue-700 font-semibold py-2.5 rounded-xl text-xs transition-colors cursor-pointer"
        >
          Ver Agenda Individual
        </button>
      </div>
    </div>

    <!-- Doctor Agenda Modal -->
    <div v-if="selectedDoctor" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm">
      <div class="bg-white rounded-3xl shadow-2xl w-full max-w-lg overflow-hidden border border-gray-100">
        <div class="bg-gray-900 px-6 py-4 text-white flex items-center justify-between">
          <div>
            <h3 class="font-bold text-base">{{ selectedDoctor.name }}</h3>
            <p class="text-xs text-gray-400">{{ selectedDoctor.specialty }} • {{ selectedDoctor.consultorio }}</p>
          </div>
          <button @click="selectedDoctor = null" class="text-gray-400 hover:text-white">✕</button>
        </div>
        <div class="p-6 space-y-4">
          <h4 class="font-bold text-gray-900 text-sm">Citas Programadas para Hoy</h4>
          <div class="space-y-2 max-h-60 overflow-y-auto">
            <div class="p-3 bg-gray-50 rounded-xl text-xs flex justify-between items-center">
              <div>
                <p class="font-bold text-gray-900">09:00 AM - María González</p>
                <p class="text-gray-500">Consulta de rutina</p>
              </div>
              <span class="px-2 py-0.5 bg-emerald-100 text-emerald-800 rounded font-semibold text-[10px]">Atendido</span>
            </div>
            <div class="p-3 bg-gray-50 rounded-xl text-xs flex justify-between items-center">
              <div>
                <p class="font-bold text-gray-900">10:00 AM - Juan Pérez</p>
                <p class="text-gray-500">Control de presión</p>
              </div>
              <span class="px-2 py-0.5 bg-blue-100 text-blue-800 rounded font-semibold text-[10px]">En consulta</span>
            </div>
            <div class="p-3 bg-gray-50 rounded-xl text-xs flex justify-between items-center">
              <div>
                <p class="font-bold text-gray-900">11:30 AM - Lucía Torres</p>
                <p class="text-gray-500">Evaluación cardiológica</p>
              </div>
              <span class="px-2 py-0.5 bg-amber-100 text-amber-800 rounded font-semibold text-[10px]">En espera</span>
            </div>
          </div>
          <button @click="selectedDoctor = null" class="w-full bg-blue-600 text-white font-bold py-2.5 rounded-xl text-sm mt-4">
            Cerrar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
