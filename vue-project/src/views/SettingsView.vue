<script setup>
import { ref } from 'vue'
import BaseButton from '@/components/BaseButton.vue'

const activeTab = ref('notificaciones')

const settingsTabs = [
  { id: 'notificaciones', label: 'Notificaciones' },
  { id: 'seguridad', label: 'Seguridad' },
  { id: 'preferencias', label: 'Preferencias' },
  { id: 'privacidad', label: 'Privacidad' }
]
</script>

<template>
  <div class="space-y-6 pb-12 max-w-4xl">
    <div>
      <h2 class="text-2xl font-bold text-gray-900">Configuración General</h2>
      <p class="text-sm text-gray-500">Administra las preferencias de tu cuenta, notificaciones y seguridad.</p>
    </div>

    <!-- Tabs -->
    <div class="flex space-x-2 border-b border-gray-200 overflow-x-auto pb-px">
      <button 
        v-for="tab in settingsTabs" 
        :key="tab.id"
        @click="activeTab = tab.id"
        :class="[
          'px-4 py-2.5 font-semibold text-sm border-b-2 transition-colors whitespace-nowrap cursor-pointer',
          activeTab === tab.id 
            ? 'border-emerald-600 text-emerald-600' 
            : 'border-transparent text-gray-500 hover:text-gray-900'
        ]"
      >
        {{ tab.label }}
      </button>
    </div>

    <!-- Tab Content: Notificaciones -->
    <div v-if="activeTab === 'notificaciones'" class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 space-y-6">
      <h3 class="text-lg font-bold text-gray-900">Preferencias de Notificaciones</h3>
      <div class="space-y-4">
        <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
          <div>
            <p class="font-semibold text-sm text-gray-900">Recordatorios de citas</p>
            <p class="text-xs text-gray-500">Recibe alertas 24 horas antes de cada consulta.</p>
          </div>
          <input type="checkbox" checked class="w-5 h-5 accent-emerald-600 rounded" />
        </label>
        <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
          <div>
            <p class="font-semibold text-sm text-gray-900">Notificaciones por correo</p>
            <p class="text-xs text-gray-500">Confirmaciones y actualizaciones enviadas a tu email.</p>
          </div>
          <input type="checkbox" checked class="w-5 h-5 accent-emerald-600 rounded" />
        </label>
        <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
          <div>
            <p class="font-semibold text-sm text-gray-900">Notificaciones Push</p>
            <p class="text-xs text-gray-500">Alertas instantáneas en tu dispositivo móvil.</p>
          </div>
          <input type="checkbox" class="w-5 h-5 accent-emerald-600 rounded" />
        </label>
        <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
          <div>
            <p class="font-semibold text-sm text-gray-900">Recordatorios de medicamentos</p>
            <p class="text-xs text-gray-500">Avisos para toma de recetas médicas activas.</p>
          </div>
          <input type="checkbox" checked class="w-5 h-5 accent-emerald-600 rounded" />
        </label>
      </div>
    </div>

    <!-- Tab Content: Seguridad -->
    <div v-if="activeTab === 'seguridad'" class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 space-y-6">
      <h3 class="text-lg font-bold text-gray-900">Seguridad de la Cuenta</h3>
      <div class="space-y-4">
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Contraseña Actual</label>
          <input type="password" placeholder="••••••••" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Nueva Contraseña</label>
          <input type="password" placeholder="••••••••" class="w-full px-4 py-2.5 rounded-xl border border-gray-300" />
        </div>
        <BaseButton buttonText="Actualizar Contraseña" />
        
        <div class="border-t border-gray-100 pt-6 mt-6">
          <div class="flex items-center justify-between">
            <div>
              <p class="font-bold text-sm text-gray-900">Autenticación de Dos Factores (2FA)</p>
              <p class="text-xs text-gray-500">Protege tu cuenta con un código de verificación adicional.</p>
            </div>
            <BaseButton buttonText="Activar 2FA" variant="outline" size="sm" />
          </div>
        </div>
      </div>
    </div>

    <!-- Tab Content: Preferencias -->
    <div v-if="activeTab === 'preferencias'" class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 space-y-6">
      <h3 class="text-lg font-bold text-gray-900">Preferencias del Sistema</h3>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Idioma</label>
          <select class="w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white">
            <option>Español (Latinoamérica)</option>
            <option>English (US)</option>
          </select>
        </div>
        <div>
          <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Zona Horaria</label>
          <select class="w-full px-4 py-2.5 rounded-xl border border-gray-300 bg-white">
            <option>GMT-3 (Buenos Aires / Santiago)</option>
            <option>GMT-5 (Bogotá / Ciudad de México)</option>
          </select>
        </div>
      </div>
    </div>

    <!-- Tab Content: Privacidad -->
    <div v-if="activeTab === 'privacidad'" class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 space-y-6">
      <h3 class="text-lg font-bold text-gray-900">Privacidad y Datos Médicos</h3>
      <div class="space-y-4">
        <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
          <div>
            <p class="font-semibold text-sm text-gray-900">Compartir información médica con especialistas</p>
            <p class="text-xs text-gray-500">Permite que tus médicos asignados consulten tu historial.</p>
          </div>
          <input type="checkbox" checked class="w-5 h-5 accent-emerald-600 rounded" />
        </label>
        <label class="flex items-center justify-between p-3 bg-gray-50 rounded-xl cursor-pointer">
          <div>
            <p class="font-semibold text-sm text-gray-900">Uso de datos anónimos para investigación</p>
            <p class="text-xs text-gray-500">Colabora con estadísticas de salud pública sin datos personales.</p>
          </div>
          <input type="checkbox" class="w-5 h-5 accent-emerald-600 rounded" />
        </label>
      </div>
    </div>
  </div>
</template>
