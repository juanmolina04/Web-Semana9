<template>
  <div class="app-container">
    <h1 class="title">Gestor de Tareas</h1>

    <!-- Campo para agregar tareas -->
    <div class="input-container">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        type="text"
        placeholder="Escribe una nueva tarea..."
      />
      <button @click="addTask">Agregar</button>
    </div>

    <!-- Lista de tareas -->
    <div v-if="hasTasks" class="task-board">
      <!-- To Do -->
      <section class="task-section todo">
        <h2>📝 To Do</h2>
        <div v-if="todo.length">
          <div
            v-for="(task, index) in todo"
            :key="'todo-' + index"
            class="task-item"
          >
            <span>{{ task }}</span>
            <button class="next-btn" @click="moveTask('todo', index)">➡️</button>
          </div>
        </div>
        <p v-else class="empty-msg">Sin tareas aquí.</p>
      </section>

      <!-- Doing -->
      <section class="task-section doing">
        <h2>⚙️ Doing</h2>
        <div v-if="doing.length">
          <div
            v-for="(task, index) in doing"
            :key="'doing-' + index"
            class="task-item"
          >
            <span>{{ task }}</span>
            <button class="next-btn" @click="moveTask('doing', index)">➡️</button>
          </div>
        </div>
        <p v-else class="empty-msg">Sin tareas aquí.</p>
      </section>

      <!-- Done -->
      <section class="task-section done">
        <h2>✅ Done</h2>
        <div v-if="done.length">
          <div
            v-for="(task, index) in done"
            :key="'done-' + index"
            class="task-item"
          >
            <span>{{ task }}</span>
          </div>
        </div>
        <p v-else class="empty-msg">Sin tareas completadas.</p>
      </section>
    </div>

    <!-- Mensaje si no hay tareas -->
    <p v-else class="no-tasks">No hay tareas registradas.</p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const newTask = ref("");
const todo = ref([]);
const doing = ref([]);
const done = ref([]);

const hasTasks = computed(
  () => todo.value.length || doing.value.length || done.value.length
);

function addTask() {
  const name = newTask.value.trim();
  if (!name) return;
  todo.value.push(name);
  newTask.value = "";
}

function moveTask(section, index) {
  if (section === "todo") {
    doing.value.push(todo.value[index]);
    todo.value.splice(index, 1);
  } else if (section === "doing") {
    done.value.push(doing.value[index]);
    doing.value.splice(index, 1);
  }
}
</script>

<style scoped>
/* Contenedor general */
.app-container {
  min-height: 100vh;
  background-color: #f4f7fa;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px 20px;
  font-family: "Segoe UI", sans-serif;
}

/* Título */
.title {
  font-size: 2rem;
  color: #2a4d9b;
  margin-bottom: 30px;
}

/* Input y botón */
.input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 25px;
  width: 100%;
  max-width: 400px;
}

input {
  flex: 1;
  padding: 8px 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  outline: none;
  transition: 0.2s;
}

input:focus {
  border-color: #2a4d9b;
}

button {
  padding: 8px 16px;
  background-color: #2a4d9b;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background-color: #1f3570;
}

/* Tablero de tareas */
.task-board {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  width: 100%;
  max-width: 1000px;
}

/* Secciones */
.task-section {
  flex: 1;
  min-width: 280px;
  background: rgb(255, 255, 255);
  border-radius: 10px;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.1);
  padding: 15px;
}

.task-section h2 {
  font-size: 1.2rem;
  margin-bottom: 10px;
  border-bottom: 2px solid #eee;
  padding-bottom: 6px;
}

/* Colores de secciones */
.todo {
  border-top: 4px solid #007bff;
}

.doing {
  border-top: 4px solid #ffc107;
}

.done {
  border-top: 4px solid #28a745;
}

/* Tareas */
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f8f9fa;
  padding: 8px 10px;
  border-radius: 6px;
  margin-bottom: 8px;
  transition: 0.2s;
}

.task-item:hover {
  background-color: #eef1f4;
}

/* Botón mover */
.next-btn {
  background: none;
  border: none;
  font-size: 1.1rem;
  cursor: pointer;
}

/* Mensajes */
.no-tasks {
  margin-top: 40px;
  color: #777;
  font-size: 1.1rem;
}

.empty-msg {
  color: #999;
  font-size: 0.9rem;
  text-align: center;
}
</style>

