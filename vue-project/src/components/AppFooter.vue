<script setup>
</script>

<template>
  <footer class="bg-gray-900 text-gray-300 pt-16 pb-12 border-t border-gray-800">
    <div class="max-w-screen-xl mx-auto px-4 lg:px-6">
      <div class="grid grid-cols-1 md:grid-cols-4 gap-10 mb-12">
        <div class="space-y-4">
          <div class="flex items-center space-x-3">
            <img src="@/assets/logoMI.png" class="h-8 w-auto" alt="Logo MiTurno" />
            <span class="text-xl font-bold tracking-tight text-white">MiTurno</span>
          </div>
          <p class="text-sm text-gray-400 leading-relaxed">
            Tu salud, nuestra prioridad. Plataforma líder en gestión de turnos médicos y experiencias de salud sin complicaciones.
          </p>
          <div class="flex space-x-4 text-emerald-400">
            <a href="#" class="hover:text-white transition-colors">
              <span class="sr-only">Twitter</span>
              🐦
            </a>
            <a href="#" class="hover:text-white transition-colors">
              <span class="sr-only">LinkedIn</span>
              💼
            </a>
            <a href="#" class="hover:text-white transition-colors">
              <span class="sr-only">GitHub</span>
              🐱
            </a>
          </div>
        </div>

        <div>
          <h4 class="text-white font-semibold mb-4 text-sm uppercase tracking-wider">Especialidades</h4>
          <ul class="space-y-2.5 text-sm">
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Cardiología</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Pediatría</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Dermatología</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Medicina General</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Odontología</a></li>
          </ul>
        </div>

        <div>
          <h4 class="text-white font-semibold mb-4 text-sm uppercase tracking-wider">Enlaces Útiles</h4>
          <ul class="space-y-2.5 text-sm">
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Sobre Nosotros</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Nuestros Profesionales</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Preguntas Frecuentes</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Política de Privacidad</a></li>
            <li><a href="#" class="hover:text-emerald-400 transition-colors">Términos y Condiciones</a></li>
          </ul>
        </div>

        <div>
          <h4 class="text-white font-semibold mb-4 text-sm uppercase tracking-wider">Contacto y Soporte</h4>
          <ul class="space-y-2.5 text-sm text-gray-400">
            <li>📍 Av. Libertador 1420, Buenos Aires</li>
            <li>📞 +54 11 5555-8877</li>
            <li>✉️ soporte@miturno.com</li>
            <li>🕒 Lun - Sáb: 8:00 - 20:00</li>
          </ul>
        </div>
      </div>

      <div class="pt-8 border-t border-gray-800 flex flex-col sm:flex-row items-center justify-between text-xs text-gray-500">
        <p>&copy; 2026 MiTurno. Todos los derechos reservados.</p>
        <p class="mt-4 sm:mt-0">Diseñado con excelencia para la salud digital.</p>
      </div>
    </div>
  </footer>
</template>
