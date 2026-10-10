<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add'])
const title = ref('')
const priority = ref('media')
const category = ref('Personal')

function submit() {
  const cleaned = title.value.trim()
  if (!cleaned) return
  emit('add', { title: cleaned, priority: priority.value, category: category.value })
  title.value = ''
}
</script>

<template>
  <form class="task-form" @submit.prevent="submit">
    <label class="field-label" for="task-title">Nueva tarea</label>
    <div class="task-entry">
      <input id="task-title" v-model="title" maxlength="100" placeholder="Ej. Terminar proyecto de Vue" required />
      <button class="primary-button" type="submit">+ Agregar tarea</button>
    </div>
    <div class="form-extras">
      <label>Prioridad
        <select v-model="priority"><option value="baja">Baja</option><option value="media">Media</option><option value="alta">Alta</option></select>
      </label>
      <label>Categoría
        <select v-model="category"><option>Personal</option><option>Estudio</option><option>Trabajo</option></select>
      </label>
    </div>
  </form>
</template>
