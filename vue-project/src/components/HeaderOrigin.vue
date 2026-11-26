<template>
  <header class="header">
    <img src="@/assets/logoMI.png" alt="Logo MiTurno" width="50" class="logo" />

    <div class="head">
      <h1>Mi Turno</h1>
      <p>Tu salud, nuestra prioridad</p>
    </div>

    <nav class="acciones">
      <button class="menu-toggle" id="menu-toggle" aria-label="Menú">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </button>

      <ul id="nav-links">
        <li><a href="#">Inicio</a></li>
        <li><a href="#">Acerca de</a></li>
        <li><a href="#">Contáctanos</a></li>

        <li class="registro">
          <button type="button">Regístrate</button>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script setup>
defineOptions({
  name: 'HeaderOrigin'
})
</script>

<style scoped>
/* Barra de navegacion original */
.header {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  color: #00d5a0;
  z-index: 1000;
  margin: 1% 5%;
  gap: 1rem;
}

.head p {
  color: #959595;
  overflow: hidden;
  border-right: 0.15em solid #00ffae;
  white-space: nowrap;
  letter-spacing: 0.1em;
  animation: typing 4s steps(30, end),
             blink 0.7s step-end infinite;
}

@keyframes typing {
  from { width: 0 }
  to { width: 100% }
}

@keyframes blink {
  50% { border-color: transparent }
}

.acciones {
  margin-left: auto;
  padding-right: 5%;
}

.acciones ul {
  display: flex;
  list-style: none;
  gap: 20px;
  align-items: center;
}

.acciones a {
  color: #333;
  text-decoration: none;
  font-weight: 500;
}

.registro button {
  color: white;
  background-color: #282828;
  border: 2px solid #02ffb3;
  border-radius: 10px;
  padding: 8px 16px;
  cursor: pointer;
}

.menu-toggle {
  display: none;
}
</style>
