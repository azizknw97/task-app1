<script setup>
defineProps({
  task: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['toggle', 'delete'])
</script>

<template>
  <li class="task-item">
    <input
      type="checkbox"
      :checked="task.done"
      @change="emit('toggle', task.id, $event.target.checked)"
    />

    <span :class="{ done: task.done }">
      {{ task.title }}
    </span>

    <button
      class="delete-button"
      @click="emit('delete', task.id)"
    >
      Hapus
    </button>
  </li>
</template>
