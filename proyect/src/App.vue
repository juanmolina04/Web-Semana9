<template>
  <div class="container">
    <h1>Gestor de Estudiantes</h1>

    <!-- Input con v-model -->
    <input v-model="newStudent" placeholder="Escribe el nombre del estudiante" @keyup.enter="addStudent" />
    <button @click="addStudent">Agregar</button>

    <hr />

    <!-- v-if / v-else -->
    <div v-if="students.length > 0">
      <h2>Lista de estudiantes</h2>
      <!-- v-for -->
      <ul>
        <li v-for="(student, index) in students" :key="index">
          {{ index + 1 }}. {{ student }}
          <button @click="removeStudent(index)">X</button>
        </li>
      </ul>
      <p>Total de estudiantes: {{ students.length }}</p>
    </div>

    <div v-else>
      <p>No hay estudiantes registrados.</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Modelo: variables reactivas
const newStudent = ref('')
const students = ref([])

// ViewModel: lógica del componente
const addStudent = () => {
  const name = newStudent.value.trim()
  if (name !== '') {
    students.value.push(name)
    newStudent.value = ''
  }
}

const removeStudent = (index) => {
  students.value.splice(index, 1)
}
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
}

input {
  padding: 8px;
  width: 70%;
}

button {
  margin-left: 8px;
  padding: 8px 12px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  background: #34f534ff;
  padding: 6px 10px;
  border-radius: 4px;
}
</style>