<script setup>
defineProps({ task: { type: Object, required: true } })
const emit = defineEmits(['toggle', 'remove'])
</script>

<template>
  <article class="task-item" :class="{ 'is-done': task.done }">
    <label class="task-check">
      <input type="checkbox" :checked="task.done" :aria-label="`Marcar ${task.title} como ${task.done ? 'pendiente' : 'completada'}`" @change="emit('toggle', task.id)" />
      <span class="task-title">{{ task.title }}</span>
    </label>
    <div class="task-metadata">
      <span class="category-tag">{{ task.category }}</span>
      <span class="priority-tag" :class="`priority-${task.priority}`">{{ task.priority }}</span>
      <button class="icon-button" type="button" :aria-label="`Eliminar ${task.title}`" title="Eliminar tarea" @click="emit('remove', task.id)">✕</button>
    </div>
  </article>
</template>
