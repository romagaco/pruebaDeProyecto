<template>
  <div class="stopwatch-container">
    <div classs="timer-display">
      {{ formattedTime }}
    </div>

    <div class="controls">
      <button v-if="!isRunning" @click="startTimer">
        Start
      </button>
      <button v-else @click="stopTimer">
        Stop
      </button>
      <button @click="resetTimer">
        Reset
      </button>
    </div>
  </div>

  <section>
    <div>
      <h2>Lista de frutas</h2>
      <ul>
        <li v-for="fruta in arrayFrutas" :key="fruta.name">
          {{fruta.price}} -- {{ fruta.name }}, {{ fruta.forma }}</li>
      </ul>
    </div>
  </section>
</template>

<script setup>

const arrayFrutas = [
  {
    name: "Manzana",
    price:"$1.00",
    forma: "redonda",
  },
  {
    name: "piña",
    price:"$2.00",
    forma: "ovalada",
  },
  {
    name: "Naranja",
    price:"$4.00",
    forma: "redonda",
  },
]






import { ref, computed } from 'vue';

// =================================================
// 1. ESTADO REACTIVO
// =================================================

// 'ref()' hace que estas variables sean reactivas.
// Cuando cambian, el HTML se actualiza automáticamente.

// Tiempo transcurrido en milisegundos
const elapsedTime = ref(0); 

// Estado del cronómetro (corriendo o pausado)
const isRunning = ref(false);

// Variable para almacenar el ID del intervalo (no necesita ser reactiva)
let timerInterval = null;

// =================================================
// 2. PROPIEDAD COMPUTADA (Formatted Time)
// =================================================

// 'computed()' crea un valor reactivo que se recalcula
// automáticamente cuando cambia 'elapsedTime'.
const formattedTime = computed(() => {
  const totalMilliseconds = elapsedTime.value;
  
  // Convertimos milisegundos a minutos, segundos y centisegundos
  const minutes = Math.floor(totalMilliseconds / 60000);
  const seconds = Math.floor((totalMilliseconds % 60000) / 1000);
  const centiseconds = Math.floor((totalMilliseconds % 1000) / 10);

  // Formateamos para que siempre tengan dos dígitos (ej: 05:12:03)
  const paddedMinutes = String(minutes).padStart(2, '0');
  const paddedSeconds = String(seconds).padStart(2, '0');
  const paddedCentiseconds = String(centiseconds).padStart(2, '0');

  return `${paddedMinutes}:${paddedSeconds}:${paddedCentiseconds}`;
});

// =================================================
// 3. MÉTODOS (Acciones)
// =================================================

function startTimer() {
  // Evitar múltiples intervalos si ya está corriendo
  if (isRunning.value) return; 

  isRunning.value = true;
  
  // Guardamos el ID del intervalo para poder detenerlo después.
  // Actualizamos el tiempo cada 10ms (1 centisegundo).
  timerInterval = setInterval(() => {
    elapsedTime.value += 10;
  }, 10);
}

function stopTimer() {
  isRunning.value = false;
  
  // Detenemos el intervalo usando el ID que guardamos
  clearInterval(timerInterval); 
}

function resetTimer() {
  // Detenemos el timer (si está corriendo)
  isRunning.value = false;
  clearInterval(timerInterval); 
  
  // Reseteamos el tiempo a 0
  elapsedTime.value = 0;
}
</script>

<style scoped>
/* 'scoped' significa que estos estilos solo aplican a este componente */

.stopwatch-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #f0f0f0;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.timer-display {
  font-size: 3rem;
  font-weight: 700;
  font-family: 'Courier New', Courier, monospace;
  color: #333;
  margin-bottom: 20px;
}

.controls {
  display: flex;
  gap: 15px;
}

button {
  font-size: 1rem;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #0056b3;
}

button:nth-child(2) { /* Botón Stop/Reset */
  background-color: #dc3545; /* Rojo para Stop */
}

button:nth-child(2):hover {
  background-color: #c82333;
}

button:nth-child(3) { /* Botón Reset */
  background-color: #6c757d; /* Gris para Reset */
}

button:nth-child(3):hover {
  background-color: #5a6268;
}
</style>