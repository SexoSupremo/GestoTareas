<template>
    <div class="card mb-4">
      <div class="card-body">
        <h5 class="card-title">Nueva Tarea</h5>
        <form @submit.prevent="handleSubmit">
          <div class="mb-3">
            <label for="taskTitle" class="form-label">Título</label>
            <input 
              type="text" 
              class="form-control" 
              id="taskTitle"
              v-model="newTask.title"
              required
              placeholder="Ingresa el título de la tarea"
            >
          </div>
          <button type="submit" class="btn btn-primary">
            <i class="bi bi-plus-circle me-2"></i>
            Agregar Tarea
          </button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TaskForm',
    data() {
      return {
        newTask: {
          title: '',
          completed: false
        }
      }
    },
    methods: {
      handleSubmit() {
        if (this.newTask.title.trim()) {
          this.$emit('task-added', { ...this.newTask })
          this.newTask.title = ''
        }
      }
    }
  }
  </script>