
<script setup>
import { ref, computed, watch } from 'vue'
import TaskItem from './components/TaskItem.vue'
import './style.css'

// 1. State Management: ref()
const newTask = ref('')
const tasks = ref([
  { id: 1, title: 'Belajar Vue.js', done: false },
  { id: 2, title: 'Mengerjakan tugas', done: true }
])

// 2. Computed Properties
const totalTasks = computed(() => tasks.value.length)

const completedTasks = computed(() =>
  tasks.value.filter(task => task.done).length
)

const pendingTasks = computed(() =>
  totalTasks.value - completedTasks.value
)

// 3. Watchers
watch(totalTasks, (newValue, oldValue) => {
  console.log(`Jumlah tugas: ${oldValue} -> ${newValue}`)
})

// 4. Event Handling: tambah tugas
function addTask() {
  const title = newTask.value.trim()

  if (!title) return

  tasks.value.push({
    id: Date.now(),
    title,
    done: false
  })

  newTask.value = ''
}

// 5. Hapus tugas
function deleteTask(id) {
  tasks.value = tasks.value.filter(task => task.id !== id)
}

// 6. Ubah status tugas
function toggleTask(id, done) {
  const task = tasks.value.find(task => task.id === id)

  if (task) {
    task.done = done
  }
}
</script>

<template>
  <main class="container">
    <h1>Task Management App</h1>
    <p class="subtitle">Kelola tugas harianmu dengan mudah.</p>

    <!-- Data Binding dan Event Handling -->
    <form class="task-form" @submit.prevent="addTask">
      <input
        v-model="newTask"
        placeholder="Masukkan nama tugas"
      />
      <button type="submit">Tambah</button>
    </form>

    <!-- Computed Properties -->
    <section class="stats">
      <div class="stat-card">
        <span>Total Tugas</span>
        <strong>{{ totalTasks }}</strong>
      </div>

      <div class="stat-card">
        <span>Selesai</span>
        <strong>{{ completedTasks }}</strong>
      </div>

      <div class="stat-card">
        <span>Belum Selesai</span>
        <strong>{{ pendingTasks }}</strong>
      </div>
    </section>

    <!-- List Rendering dan Components -->
    <ul v-if="tasks.length > 0" class="task-list">
      <TaskItem
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @toggle="toggleTask"
        @delete="deleteTask"
      />
    </ul>

    <p v-else class="empty-message">
      Belum ada tugas. Tambahkan tugas baru!
    </p>
  </main>
</template>
