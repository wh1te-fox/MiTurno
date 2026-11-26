<script setup>
</script>

<template>
  <div class="space-y-6 pb-12">
    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
      <div>
        <h2 class="text-2xl font-bold text-gray-900">Reportes y Estadísticas Clínicas</h2>
        <p class="text-sm text-gray-500">Métricas operativas, rendimiento médico y asistencia de pacientes.</p>
      </div>
      <div class="flex space-x-2">
        <button class="bg-gray-100 hover:bg-gray-200 text-gray-700 font-semibold px-4 py-2 rounded-xl text-xs transition-colors">
          Exportar PDF
        </button>
        <button class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-4 py-2 rounded-xl text-xs transition-colors">
          Exportar Excel
        </button>
      </div>
    </div>

    <!-- Summary Metrics -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
      <div class="bg-white p-6 rounded-3xl shadow-xs border border-gray-100">
        <p class="text-xs font-semibold text-gray-500 uppercase">Tasa de Asistencia</p>
        <h3 class="text-3xl font-black text-emerald-600 mt-2">94.2%</h3>
        <p class="text-xs text-emerald-700 mt-1">↑ +2.1% vs mes anterior</p>
      </div>
      <div class="bg-white p-6 rounded-3xl shadow-xs border border-gray-100">
        <p class="text-xs font-semibold text-gray-500 uppercase">Total Citas del Mes</p>
        <h3 class="text-3xl font-black text-gray-900 mt-2">648</h3>
        <p class="text-xs text-blue-600 mt-1">82 citas/día promedio</p>
      </div>
      <div class="bg-white p-6 rounded-3xl shadow-xs border border-gray-100">
        <p class="text-xs font-semibold text-gray-500 uppercase">Tiempo Promedio de Espera</p>
        <h3 class="text-3xl font-black text-amber-500 mt-2">14 min</h3>
        <p class="text-xs text-emerald-600 mt-1">↓ -3 min de optimización</p>
      </div>
      <div class="bg-white p-6 rounded-3xl shadow-xs border border-gray-100">
        <p class="text-xs font-semibold text-gray-500 uppercase">Nuevos Pacientes</p>
        <h3 class="text-3xl font-black text-blue-600 mt-2">+124</h3>
        <p class="text-xs text-gray-500 mt-1">Registrados este mes</p>
      </div>
    </div>

    <!-- Analytics Breakdown -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <div class="bg-white p-6 rounded-3xl shadow-xs border border-gray-100 space-y-4">
        <h3 class="font-bold text-gray-900 text-base">Especialidades Más Solicitadas</h3>
        <div class="space-y-3">
          <div>
            <div class="flex justify-between text-xs font-semibold text-gray-700 mb-1">
              <span>Cardiología</span>
              <span>32%</span>
            </div>
            <div class="w-full bg-gray-100 h-2.5 rounded-full overflow-hidden">
              <div class="bg-blue-600 h-2.5 rounded-full" style="width: 32%"></div>
            </div>
          </div>
          <div>
            <div class="flex justify-between text-xs font-semibold text-gray-700 mb-1">
              <span>Pediatría</span>
              <span>28%</span>
            </div>
            <div class="w-full bg-gray-100 h-2.5 rounded-full overflow-hidden">
              <div class="bg-emerald-500 h-2.5 rounded-full" style="width: 28%"></div>
            </div>
          </div>
          <div>
            <div class="flex justify-between text-xs font-semibold text-gray-700 mb-1">
              <span>Dermatología</span>
              <span>18%</span>
            </div>
            <div class="w-full bg-gray-100 h-2.5 rounded-full overflow-hidden">
              <div class="bg-purple-500 h-2.5 rounded-full" style="width: 18%"></div>
            </div>
          </div>
          <div>
            <div class="flex justify-between text-xs font-semibold text-gray-700 mb-1">
              <span>Oftalmología</span>
              <span>14%</span>
            </div>
            <div class="w-full bg-gray-100 h-2.5 rounded-full overflow-hidden">
              <div class="bg-amber-500 h-2.5 rounded-full" style="width: 14%"></div>
            </div>
          </div>
        </div>
      </div>

      <div class="bg-white p-6 rounded-3xl shadow-xs border border-gray-100 space-y-4">
        <h3 class="font-bold text-gray-900 text-base">Distribución por Tipo de Consulta</h3>
        <div class="grid grid-cols-2 gap-4 pt-2">
          <div class="bg-blue-50 p-4 rounded-2xl text-center">
            <span class="text-2xl">🏥</span>
            <h4 class="text-2xl font-black text-blue-900 mt-2">78%</h4>
            <p class="text-xs text-blue-700 font-semibold">Consultas Presenciales</p>
          </div>
          <div class="bg-emerald-50 p-4 rounded-2xl text-center">
            <span class="text-2xl">💻</span>
            <h4 class="text-2xl font-black text-emerald-900 mt-2">22%</h4>
            <p class="text-xs text-emerald-700 font-semibold">Telemedicina / Virtual</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
