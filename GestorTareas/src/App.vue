<template>
  <div class="min-h-screen bg-gray-100 flex flex-col items-center p-8">
    <h1 class="text-3xl font-bold mb-6 text-blue-700">Gestor de Tareas</h1>

    <!-- Campo para agregar tareas -->
    <div class="flex gap-2 mb-6 w-full max-w-md">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        type="text"
        placeholder="Escribe una nueva tarea..."
        class="flex-1 p-2 border rounded-lg outline-none focus:ring-2 focus:ring-blue-500"
      />
      <button
        @click="addTask"
        class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition"
      >
        Agregar
      </button>
    </div>

    <!-- Lista de tareas -->
    <div v-if="hasTasks" class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full max-w-5xl">
      <!-- Sección To Do -->
      <section class="bg-white shadow-lg rounded-xl p-4">
        <h2 class="text-xl font-semibold mb-3 text-gray-700 border-b pb-2">📝 To Do</h2>
        <div v-if="todo.length" class="space-y-2">
          <div
            v-for="(task, index) in todo"
            :key="'todo-' + index"
            class="flex justify-between items-center bg-gray-50 p-2 rounded-lg"
          >
            <span>{{ task }}</span>
            <button
              @click="moveTask('todo', index)"
              class="text-green-600 hover:text-green-800 font-semibold"
            >
              ➡️
            </button>
          </div>
        </div>
        <p v-else class="text-gray-400 text-sm">Sin tareas en esta sección.</p>
      </section>

      <!-- Sección Doing -->
      <section class="bg-white shadow-lg rounded-xl p-4">
        <h2 class="text-xl font-semibold mb-3 text-gray-700 border-b pb-2">⚙️ Doing</h2>
        <div v-if="doing.length" class="space-y-2">
          <div
            v-for="(task, index) in doing"
            :key="'doing-' + index"
            class="flex justify-between items-center bg-yellow-50 p-2 rounded-lg"
          >
            <span>{{ task }}</span>
            <button
              @click="moveTask('doing', index)"
              class="text-green-600 hover:text-green-800 font-semibold"
            >
              ➡️
            </button>
          </div>
        </div>
        <p v-else class="text-gray-400 text-sm">Sin tareas en esta sección.</p>
      </section>

      <!-- Sección Done -->
      <section class="bg-white shadow-lg rounded-xl p-4">
        <h2 class="text-xl font-semibold mb-3 text-gray-700 border-b pb-2">✅ Done</h2>
        <div v-if="done.length" class="space-y-2">
          <div
            v-for="(task, index) in done"
            :key="'done-' + index"
            class="flex justify-between items-center bg-green-50 p-2 rounded-lg"
          >
            <span>{{ task }}</span>
          </div>
        </div>
        <p v-else class="text-gray-400 text-sm">Sin tareas completadas.</p>
      </section>
    </div>

    <!-- Mensaje si no hay tareas -->
    <p v-else class="text-gray-500 text-lg mt-10">No hay tareas registradas.</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref('')
const todo = ref([])
const doing = ref([])
const done = ref([])

const hasTasks = computed(() => todo.value.length || doing.value.length || done.value.length)

function addTask() {
  const taskName = newTask.value.trim()
  if (!taskName) return
  todo.value.push(taskName)
  newTask.value = ''
}

function moveTask(section, index) {
  if (section === 'todo') {
    doing.value.push(todo.value[index])
    todo.value.splice(index, 1)
  } else if (section === 'doing') {
    done.value.push(doing.value[index])
    doing.value.splice(index, 1)
  }
  // Las tareas en Done no se eliminan ni se mueven
}
</script>

<style>
body {
  font-family: "Inter", sans-serif;
}
</style>
