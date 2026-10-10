<script setup>
import { computed, ref, watch } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskItem from './components/TaskItem.vue'

const storageKey = 'taskflow-tasks-v1'
function initialTasks() {
  try {
    const saved = localStorage.getItem(storageKey)
    if (saved !== null) {
      const parsed = JSON.parse(saved)
      if (Array.isArray(parsed)) return parsed.filter(t => t && typeof t.id === 'string' && typeof t.title === 'string' && typeof t.done === 'boolean')
    }
  } catch (error) { console.warn('No se pudieron recuperar las tareas guardadas:', error) }
  return [
    { id: 'example-1', title: 'Revisar la documentación de Vue', category: 'Estudio', priority: 'alta', done: true },
    { id: 'example-2', title: 'Terminar la práctica de componentes', category: 'Estudio', priority: 'alta', done: false },
    { id: 'example-3', title: 'Organizar actividades de la semana', category: 'Personal', priority: 'media', done: false }
  ]
}

const tasks = ref(initialTasks())
const activeFilter = ref('todas')
const search = ref('')
const filters = [ { value: 'todas', label: 'Todas' }, { value: 'pendientes', label: 'Pendientes' }, { value: 'completadas', label: 'Completadas' } ]

const total = computed(() => tasks.value.length)
const doneCount = computed(() => tasks.value.filter(task => task.done).length)
const pendingCount = computed(() => total.value - doneCount.value)
const progress = computed(() => total.value ? Math.round(doneCount.value / total.value * 100) : 0)
const filteredTasks = computed(() => tasks.value.filter(task => {
  const matchesStatus = activeFilter.value === 'todas' || (activeFilter.value === 'completadas' ? task.done : !task.done)
  return matchesStatus && task.title.toLocaleLowerCase('es').includes(search.value.trim().toLocaleLowerCase('es'))
}))

watch(tasks, updated => {
  try { localStorage.setItem(storageKey, JSON.stringify(updated)) }
  catch (error) { console.warn('No se pudieron guardar las tareas:', error) }
}, { deep: true })

function addTask({ title, priority, category }) {
  const id = typeof crypto !== 'undefined' && crypto.randomUUID ? crypto.randomUUID() : `${Date.now()}-${Math.random()}`
  tasks.value.unshift({ id, title, priority, category, done: false })
  activeFilter.value = 'todas'
  search.value = ''
}
function toggleTask(id) {
  const task = tasks.value.find(task => task.id === id)
  if (task) task.done = !task.done
}
function removeTask(id) { tasks.value = tasks.value.filter(task => task.id !== id) }
function clearCompleted() { tasks.value = tasks.value.filter(task => !task.done) }
</script>

<template>
  <div class="app-shell">
    <aside class="sidebar">
      <div class="brand"><div class="brand-icon">✓</div><span>TaskFlow<span class="brand-dot">.</span></span></div>
      <div class="sidebar-section"><span class="eyebrow">ESPACIO DE TRABAJO</span><div class="nav-item selected">▦ <span>Mis tareas</span></div></div>
      <div class="sidebar-help"><div class="help-icon">✦</div><strong>Pequeños pasos,<br />grandes resultados.</strong><p>Organiza tus pendientes y mantén el enfoque en lo importante.</p></div>
      <p class="sidebar-footer">Proyecto académico · Vue 3</p>
    </aside>

    <main class="main-content">
      <header class="topbar"><div class="breadcrumb">Mi espacio <span>/</span> <strong>Mis tareas</strong></div><div class="avatar">TF</div></header>
      <div class="page-content">
        <div class="page-head"><div><span class="eyebrow purple">TU PRODUCTIVIDAD EN UN SOLO LUGAR</span><h1>Organiza tu día<span class="heading-dot">.</span></h1><p>Una lista clara para enfocarte en lo que realmente importa.</p></div><div class="date-label">✦ &nbsp; Un día a la vez</div></div>

        <section class="stats-grid" aria-label="Resumen de tareas">
          <article class="stat-card"><span class="stat-label">Total de tareas</span><div class="stat-bottom"><strong>{{ total }}</strong><span class="stat-icon lavender">▦</span></div></article>
          <article class="stat-card"><span class="stat-label">Pendientes</span><div class="stat-bottom"><strong>{{ pendingCount }}</strong><span class="stat-icon amber">◷</span></div></article>
          <article class="stat-card"><span class="stat-label">Completadas</span><div class="stat-bottom"><strong>{{ doneCount }}</strong><span class="stat-icon mint">✓</span></div></article>
        </section>

        <section class="progress-card" aria-label="Progreso general"><div class="progress-heading"><div><span class="eyebrow purple">TU PROGRESO</span><h2>Vas por buen camino</h2><p>¡Cada tarea completada cuenta!</p></div><strong>{{ progress }}%</strong></div><div class="progress-track" role="progressbar" :aria-valuenow="progress" aria-valuemin="0" aria-valuemax="100" aria-label="Porcentaje completado"><div class="progress-fill" :style="{ width: `${progress}%` }"></div></div><div class="progress-note">{{ doneCount }} de {{ total }} tareas completadas</div></section>

        <section class="card add-card"><div class="section-title"><span class="section-symbol">＋</span><h2>Agregar una tarea</h2></div><TaskForm @add="addTask" /></section>

        <section class="card tasks-card"><div class="task-section-head"><div class="section-title"><span class="section-symbol">☷</span><h2>Mis tareas</h2><span class="count-pill">{{ total }}</span></div><button v-if="doneCount" class="text-button" type="button" @click="clearCompleted">Limpiar completadas</button></div>
          <div class="toolbar"><div class="filters" role="group" aria-label="Filtrar tareas"><button v-for="filter in filters" :key="filter.value" class="filter-btn" :class="{ active: activeFilter === filter.value }" type="button" :aria-pressed="activeFilter === filter.value" @click="activeFilter = filter.value">{{ filter.label }}</button></div><label class="search-box"><span aria-hidden="true">⌕</span><input v-model="search" type="search" placeholder="Buscar tarea..." aria-label="Buscar tarea" /></label></div>
          <div class="task-list"><TaskItem v-for="task in filteredTasks" :key="task.id" :task="task" @toggle="toggleTask" @remove="removeTask" /><div v-if="!filteredTasks.length" class="empty-state"><div class="empty-icon">✓</div><strong>No hay tareas aquí</strong><p>Agrega una nueva tarea o cambia los filtros.</p></div></div>
          <p class="storage-note">☁ Tus tareas se guardan automáticamente en este navegador.</p>
        </section>
        <footer class="page-footer">Hecho con Vue.js + Vite · TaskFlow</footer>
      </div>
    </main>
  </div>
</template>
