<script setup>
import { ref } from 'vue'

const activeTab = ref('clinica')

const tabs = [
  { id: 'clinica', label: 'Clínica' },
  { id: 'roles', label: 'Usuarios y Roles' },
  { id: 'especialidades', label: 'Especialidades' },
  { id: 'consultorios', label: 'Consultorios' },
  { id: 'horarios', label: 'Horarios y Feriados' },
  { id: 'notificaciones', label: 'Notificaciones' }
]

const specialtiesList = ref([
  { id: 1, name: 'Cardiología', active: true, doctors: 4 },
  { id: 2, name: 'Pediatría', active: true, doctors: 5 },
  { id: 3, name: 'Dermatología', active: true, doctors: 3 },
  { id: 4, name: 'Oftalmología', active: true, doctors: 2 },
  { id: 5, name: 'Odontología', active: false, doctors: 0 }
])
</script>

<template>
  <div class="space-y-6 pb-12 max-w-5xl">
    <div>
      <h2 class="text-2xl font-bold text-gray-900">Configuración del Sistema Clínico</h2>
      <p class="text-sm text-gray-500">Parámetros generales de la clínica, roles de personal, consultorios y especialidades.</p>
    </div>

    <!-- Navigation Tabs -->
    <div class="flex space-x-2 border-b border-gray-200 overflow-x-auto pb-px">
      <button 
        v-for="t in tabs" 
        :key="t.id"
        @click="activeTab = t.id"
        :class="[
          'px-4 py-2.5 font-semibold text-sm border-b-2 transition-colors whitespace-nowrap cursor-pointer',
          activeTab === t.id 
            ? 'border-blue-600 text-blue-600' 
            : 'border-transparent text-gray-500 hover:text-gray-900'
        ]"
      >
        {{ t.label }}
      </button>
    </div>

    <!-- Tab: Clínica -->
    <div v-if="activeTab === 'clinica'" class="bg-white p-8 rounded-3xl shadow-xs border border-gray-100 space-y-6">
      <h3 class="font-bold text-gray-900 text-lg">Información Institucional</h3>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Nombre de la Clínica</label>
          <input type="text" value="MiTurno - Clínica Central" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Dirección Principal</label>
          <input type="text" value="Av. Libertador 1420, Buenos Aires" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Teléfono Central</label>
          <input type="text" value="+54 11 5555-8877" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Correo Electrónico Oficial</label>
          <input type="email" value="contacto@miturno-clinica.com" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
        </div>
      </div>
      <button class="bg-blue-600 text-white font-semibold px-5 py-2.5 rounded-xl text-sm hover:bg-blue-700">
        Guardar Configuración
      </button>
    </div>

    <!-- Tab: Usuarios y Roles -->
    <div v-if="activeTab === 'roles'" class="bg-white p-8 rounded-3xl shadow-xs border border-gray-100 space-y-6">
      <div class="flex justify-between items-center">
        <h3 class="font-bold text-gray-900 text-lg">Gestión de Personal y Permisos</h3>
        <button class="bg-blue-600 text-white text-xs font-semibold px-3 py-2 rounded-xl">+ Nuevo Usuario</button>
      </div>
      <div class="divide-y divide-gray-100">
        <div class="py-3 flex justify-between items-center text-sm">
          <div>
            <p class="font-bold text-gray-900">Carlos Mendoza</p>
            <p class="text-xs text-gray-500">carlos.mendoza@clinica.com</p>
          </div>
          <span class="px-2.5 py-1 bg-purple-100 text-purple-700 text-xs font-bold rounded-full">Administrador General</span>
        </div>
        <div class="py-3 flex justify-between items-center text-sm">
          <div>
            <p class="font-bold text-gray-900">Lucía Torres</p>
            <p class="text-xs text-gray-500">lucia.recepcion@clinica.com</p>
          </div>
          <span class="px-2.5 py-1 bg-blue-100 text-blue-700 text-xs font-bold rounded-full">Recepcionista</span>
        </div>
        <div class="py-3 flex justify-between items-center text-sm">
          <div>
            <p class="font-bold text-gray-900">Dra. Sofía Martínez</p>
            <p class="text-xs text-gray-500">sofia.martinez@clinica.com</p>
          </div>
          <span class="px-2.5 py-1 bg-emerald-100 text-emerald-700 text-xs font-bold rounded-full">Personal Médico</span>
        </div>
      </div>
    </div>

    <!-- Tab: Especialidades -->
    <div v-if="activeTab === 'especialidades'" class="bg-white p-8 rounded-3xl shadow-xs border border-gray-100 space-y-6">
      <div class="flex justify-between items-center">
        <h3 class="font-bold text-gray-900 text-lg">Especialidades Médicas Activas</h3>
        <button class="bg-blue-600 text-white text-xs font-semibold px-3 py-2 rounded-xl">+ Añadir Especialidad</button>
      </div>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
        <div v-for="spec in specialtiesList" :key="spec.id" class="p-4 bg-gray-50 rounded-2xl flex items-center justify-between">
          <div>
            <p class="font-bold text-gray-900">{{ spec.name }}</p>
            <p class="text-xs text-gray-500">{{ spec.doctors }} médicos asignados</p>
          </div>
          <span :class="['text-xs font-semibold px-2.5 py-1 rounded-full', spec.active ? 'bg-emerald-100 text-emerald-700' : 'bg-gray-200 text-gray-600']">
            {{ spec.active ? 'Activa' : 'Inactiva' }}
          </span>
        </div>
      </div>
    </div>

    <!-- Tab: Consultorios -->
    <div v-if="activeTab === 'consultorios'" class="bg-white p-8 rounded-3xl shadow-xs border border-gray-100 space-y-6">
      <h3 class="font-bold text-gray-900 text-lg">Asignación de Consultorios</h3>
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <div class="p-4 bg-gray-50 rounded-2xl border border-gray-200">
          <p class="font-bold text-gray-900">Consultorio 101</p>
          <p class="text-xs text-gray-500 mt-1">Medicina General - Planta Baja</p>
          <span class="mt-3 inline-block text-[10px] font-bold text-emerald-600 bg-emerald-50 px-2 py-0.5 rounded">Operativo</span>
        </div>
        <div class="p-4 bg-gray-50 rounded-2xl border border-gray-200">
          <p class="font-bold text-gray-900">Consultorio 201</p>
          <p class="text-xs text-gray-500 mt-1">Pediatría - Piso 2</p>
          <span class="mt-3 inline-block text-[10px] font-bold text-emerald-600 bg-emerald-50 px-2 py-0.5 rounded">Operativo</span>
        </div>
        <div class="p-4 bg-gray-50 rounded-2xl border border-gray-200">
          <p class="font-bold text-gray-900">Consultorio 402</p>
          <p class="text-xs text-gray-500 mt-1">Cardiología - Piso 4</p>
          <span class="mt-3 inline-block text-[10px] font-bold text-emerald-600 bg-emerald-50 px-2 py-0.5 rounded">Operativo</span>
        </div>
      </div>
    </div>

    <!-- Tab: Horarios -->
    <div v-if="activeTab === 'horarios'" class="bg-white p-8 rounded-3xl shadow-xs border border-gray-100 space-y-6">
      <h3 class="font-bold text-gray-900 text-lg">Horarios de Atención Clínica</h3>
      <div class="space-y-3 text-sm">
        <div class="flex justify-between py-2 border-b border-gray-100">
          <span class="font-medium text-gray-700">Lunes a Viernes</span>
          <span class="font-bold text-gray-900">08:00 AM - 20:00 PM</span>
        </div>
        <div class="flex justify-between py-2 border-b border-gray-100">
          <span class="font-medium text-gray-700">Sábados</span>
          <span class="font-bold text-gray-900">08:00 AM - 14:00 PM</span>
        </div>
        <div class="flex justify-between py-2 border-b border-gray-100">
          <span class="font-medium text-gray-700">Domingos y Feriados</span>
          <span class="font-bold text-red-600">Guardia de Urgencias Únicamente</span>
        </div>
      </div>
    </div>

    <!-- Tab: Notificaciones -->
    <div v-if="activeTab === 'notificaciones'" class="bg-white p-8 rounded-3xl shadow-xs border border-gray-100 space-y-4">
      <h3 class="font-bold text-gray-900 text-lg">Alertas y Notificaciones Internas</h3>
      <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
        <div>
          <p class="font-semibold text-sm text-gray-900">Alerta de paciente en espera > 30 minutos</p>
          <p class="text-xs text-gray-500">Notificar a recepción y médico asignado.</p>
        </div>
        <input type="checkbox" checked class="w-5 h-5 accent-blue-600 rounded" />
      </label>
      <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
        <div>
          <p class="font-semibold text-sm text-gray-900">Recordatorio automático por WhatsApp a pacientes</p>
          <p class="text-xs text-gray-500">Enviar mensaje de confirmación 24 horas antes.</p>
        </div>
        <input type="checkbox" checked class="w-5 h-5 accent-blue-600 rounded" />
      </label>
    </div>
  </div>
</template>
