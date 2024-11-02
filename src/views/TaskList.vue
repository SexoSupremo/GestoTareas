<template>
    <div class="container py-4">
      <h2 class="text-center mb-4">Lista de Tareas</h2>
      
      <!-- Botón para cargar tareas de la API -->
      <div class="d-flex justify-content-end mb-3">
        <button 
          class="btn btn-primary"
          @click="fetchTasksFromApi"
          :disabled="loading"
        >
          <i class="bi bi-cloud-download me-2"></i>
          {{ loading ? 'Cargando...' : 'Cargar Tareas de API' }}
        </button>
      </div>
  
      <!-- Indicador de carga -->
      <div v-if="loading" class="text-center my-4">
        <div class="spinner-border text-primary" role="status">
          <span class="visually-hidden">Cargando...</span>
        </div>
      </div>
  
      <!-- Mensaje de error -->
      <div v-if="error" class="alert alert-danger" role="alert">
        {{ error }}
        <button type="button" class="btn-close float-end" @click="error = ''"></button>
      </div>
  
      <!-- Lista de tareas -->
      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <div class="card-body">
              <div v-if="tasks.length === 0 && !loading" class="text-center text-muted my-4">
                No hay tareas disponibles. Carga algunas desde la API.
              </div>
              <div v-else class="list-group">
                <div 
                  v-for="task in sortedTasks" 
                  :key="task.id"
                  class="list-group-item list-group-item-action"
                  :class="{'bg-light': task.completed}"
                >
                  <div class="d-flex justify-content-between align-items-center">
                    <div class="form-check">
                      <input
                        type="checkbox"
                        class="form-check-input"
                        :checked="task.completed"
                        @change="toggleTask(task)"
                      />
                      <label
                        class="form-check-label ms-2"
                        :class="{'text-decoration-line-through': task.completed}"
                      >
                        {{ task.todo }}
                      </label>
                    </div>
                    <div class="d-flex align-items-center">
                      <span 
                        class="badge me-2"
                        :class="task.completed ? 'bg-success' : 'bg-warning'"
                      >
                        {{ task.completed ? 'Completada' : 'Pendiente' }}
                      </span>
                      <button
                        class="btn btn-outline-danger btn-sm"
                        @click="deleteTask(task)"
                      >
                        <i class="bi bi-trash"></i>
                      </button>
                    </div>
                  </div>
                  <small class="text-muted d-block mt-1">
                    ID: {{ task.id }}
                  </small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'TaskList',
    data() {
      return {
        tasks: [],
        loading: false,
        error: ''
      };
    },
    computed: {
      sortedTasks() {
        return [...this.tasks].sort((a, b) => {
          // Ordenar primero por estado (pendientes primero)
          if (a.completed !== b.completed) {
            return a.completed ? 1 : -1;
          }
          // Luego por ID (más recientes primero)
          return b.id - a.id;
        });
      }
    },
    methods: {
      async fetchTasksFromApi() {
        this.loading = true;
        this.error = '';
        
        try {
          const response = await axios.get('https://dummyjson.com/todos');
          this.tasks = response.data.todos.map(task => ({
            ...task,
            todo: task.todo || task.title // Manejar diferentes formatos de API
          }));
        } catch (err) {
          this.error = 'Error al cargar las tareas. Por favor, intente nuevamente.';
          console.error('Error:', err);
        } finally {
          this.loading = false;
        }
      },
  
      toggleTask(task) {
        task.completed = !task.completed;
      },
  
      deleteTask(task) {
        const index = this.tasks.findIndex(t => t.id === task.id);
        if (index > -1) {
          this.tasks.splice(index, 1);
        }
      },
    }
  };
  </script>
  
  <style scoped>
  .list-group-item {
    transition: all 0.3s ease;
  }
  
  .list-group-item:hover {
    transform: translateX(5px);
  }
  
  .btn-outline-danger {
    opacity: 0.7;
  }
  
  .btn-outline-danger:hover {
    opacity: 1;
  }
  
  .form-check-label {
    cursor: pointer;
  }
  
  .badge {
    transition: all 0.3s ease;
  }
  </style>