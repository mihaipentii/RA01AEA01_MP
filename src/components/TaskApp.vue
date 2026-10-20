<script setup>
import { ref, computed } from 'vue'

// Datos reactivos
const nuevaTarea = ref('')
const soloPendientes = ref(false)
const siguienteId = ref(4)
const tareas = ref([
  { id: 1, titulo: 'Tasca 1', completada: true, fecha: new Date().toLocaleDateString() },
  { id: 2, titulo: 'Tasca 2', completada: false, fecha: new Date().toLocaleDateString() },
  { id: 3, titulo: 'Tasca 3', completada: false, fecha: new Date().toLocaleDateString() },
])

// Lista y contadores
const tareasVisibles = computed(() =>
  soloPendientes.value ? tareas.value.filter((t) => !t.completada) : tareas.value,
)
const total = computed(() => tareas.value.length)
const pendientes = computed(() => tareas.value.filter((t) => !t.completada).length)

// Metodes
function afegir() {
  const titulo = nuevaTarea.value.trim()
  if (!titulo) return
  tareas.value.push({
    id: siguienteId.value++,
    titulo,
    completada: false,
    fecha: new Date().toLocaleDateString(),
  })
  nuevaTarea.value = ''
}

function alternar(tarea) {
  tarea.completada = !tarea.completada
}

function eliminar(id) {
  tareas.value = tareas.value.filter((t) => t.id !== id)
}
</script>

<template>
  <div class="task-app">
    <h1>Gestor de Tasques</h1>

    <!-- Añadir tarea -->
    <div>
      <input v-model="nuevaTarea" type="text" placeholder="Escriu una nova tasca" @keyup.enter="afegir" />
      <button @click="afegir">Afegir</button>
    </div>

    <label>
      <input type="checkbox" v-model="soloPendientes" />
      Mostra només pendents
    </label>

    <!-- Lista -->
    <ul v-if="tareasVisibles.length">
      <!-- Cada tarea -->
      <li v-for="tarea in tareasVisibles" :key="tarea.id">
        <!-- Tarea hecha -->
        <span :class="{ completada: tarea.completada }">{{ tarea.titulo }}</span>
        <!-- Fecha -->
        <small v-show="tarea.fecha"> ({{ tarea.fecha }})</small>
        <button @click="alternar(tarea)">
          {{ tarea.completada ? 'Desmarcar' : 'Completar' }}
        </button>
        <button @click="eliminar(tarea.id)">🗑️</button>
      </li>
    </ul>
    <p v-else>No hi ha tasques per mostrar.</p>

    <p>Total: {{ total }} | Pendents: {{ pendientes }}</p>
  </div>
</template>

<style scoped>
.task-app {
  text-align: center;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin: 4px 0;
}
.completada {
  text-decoration: line-through;
  color: gray;
}
</style>