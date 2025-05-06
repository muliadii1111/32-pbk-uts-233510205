<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filter = ref('all')

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const toggleTask = (task) => {
  task.completed == !task.completed
}

const filteredTasks = computed(() => {
  if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else if (filter.value === 'active') {
    return tasks.value.filter(task => !task.completed)
  } else {
    return tasks.value
  }
})
</script>

<template>
  <div class="min-h-screen bg-blue-50 flex items-center justify-center p-6">
    <div class="bg-white rounded-2xl h-120 shadow-xl w-full max-w-3xl p-8 space-y-6">
      <h1 class="text-3xl font-bold text-blue-600 text-center">📘 Manajemen Tugas</h1>
      <div class="flex gap-3">
        <input v-model="newTask" @keyup.enter="addTask" type="text" placeholder="Tugas baru..."
          class="flex-1 px-4 py-2 border border-blue-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400" />
        <button @click="addTask" class="bg-blue-500 hover:bg-blue-600 text-white font-semibold px-4 py-2 rounded-lg">
          Tambahkan
        </button>
      </div>

      <div class="flex justify-center gap-4">
        <button @click="filter = 'all'" class="px-4 py-1 rounded-full border text-sm font-medium"
          :class="filter === 'all' ? 'bg-blue-500 text-white' : 'bg-gray-100 text-gray-700 hover:bg-gray-200'">
          Semua
        </button>
        <button @click="filter = 'completed'" class="px-4 py-1 rounded-full border text-sm font-medium"
          :class="filter === 'completed' ? 'bg-blue-500 text-white' : 'bg-gray-100 text-gray-700 hover:bg-gray-200'">
          Selesai
        </button>
        <button @click="filter = 'active'" class="px-4 py-1 rounded-full border text-sm font-medium"
          :class="filter === 'active' ? 'bg-blue-500 text-white' : 'bg-gray-100 text-gray-700 hover:bg-gray-200'">
          Aktif
        </button>
      </div>

      <ul class="space-y-3 h-60 overflow-y-auto">
        <li v-for="task in filteredTasks" :key="task.id"
          class="flex items-center justify-between bg-gray-50 px-4 py-3 rounded-lg border border-gray-200">
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" class="accent-blue-500 w-4 h-4">
            <span :class="{ 'line-through text-gray-400': task.completed }">
              {{ task.text }}
            </span>
          </div>
          <button @click="removeTask(task)" class="text-red-400 hover:text-red-600 text-lg font-bold">Hapus</button>
        </li>
      </ul>
    </div>
  </div>
</template>
