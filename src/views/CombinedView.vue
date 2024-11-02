<template>
    <div class="container py-4">
      <div class="row justify-content-center">
        <div class="col-lg-10">
          <h2 class="text-center mb-4">Vista Combinada de Tareas</h2>
          
          <!-- TaskForm Component -->
          <TaskForm @task-added="addTask" />
  
          <!-- Estado de carga y error -->
          <div v-if="loading" class="text-center my-4">
            <div class="spinner-border text-primary" role="status">
              <span class="visually-hidden">Cargando...</span>
            </div>
          </div>
  
          <div v-if="error" class="alert alert-danger alert-dismissible fade show" role="alert">
            {{ error }}
            <button type="button" class="btn-close" @click="error = ''" aria-label="Close"></button>
          </div>
  
          <!-- Filtros -->
          <div class="card mb-4">
            <div class="card-body">
              <div class="row align-items-center">
                <div class="col-md-4">
                  <select v-model="filter" class="form-select">
                    <option value="all">Todas las tareas</option>
                    <option value="pending">Pendientes</option>
                    <option value="completed">Completadas</option>
                  </select>
                </div>
                <div class="col-md-4">
                  <select v-model="source" class="form-select">
                    <option value="all">Todas las fuentes</option>
                    <option value="local">Tareas locales</option>
                    <option value="api">Tareas de API</option>
                  </select>
                </div>
                <div class="col-md-4">
                  <div class="input-group">
                    <input 
                      type="text" 
                      class="form-control" 
                      v-model="searchTerm"
                      placeholder="Buscar tareas..."
                    >
                    <span class="input-group-text">
                      <i class="bi bi-search"></i>
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </div>
  
          <!-- Lista de tareas -->
          <div class="card">
            <div class="card-body">
              <div v-if="filteredTasks.length === 0" class="text-center py-4">
                <i class="bi bi-inbox fs-1 text-muted"></i>
                <p class="text-muted mt-2">No se encontraron tareas que coincidan con los filtros.</p>
              </div>
              
              <div v-else class="list-group">
                <div 
                  v-for="task in filteredTasks" 
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
                      >
                      <label 
                        class="form-check-label ms-2"
                        :class="{'text-decoration-line-through': task.completed}"
                      >
                        {{ task.title || task.todo }}
                      </label>
                    </div>
                    <div class="d-flex align-items-center gap-2">
                      <span 
                        class="badge"
                        :class="task.isLocal ? 'bg-success' : 'bg-info'"
                      >
                        {{ task.isLocal ? 'Local' : 'API' }}
                      </span>
                      <span 
                        class="badge"
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
                </div>
              </div>
            </div>
          </div>
  
          <!-- Estadísticas -->
          <div class="card mt-4">
            <div class="card-body">
              <h5 class="card-title">Estadísticas</h5>
              <div class="row">
                <div class="col-md-3">
                  <div class="text-center">
                    <h6>Total de tareas</h6>
                    <p class="h4">{{ allTasks.length }}</p>
                  </div>
                </div>
                <div class="col-md-3">
                  <div class="text-center">
                    <h6>Completadas</h6>
                    <p class="h4 text-success">{{ completedTasks.length }}</p>
                  </div>
                </div>
                <div class="col-md-3">
                  <div class="text-center">
                    <h6>Pendientes</h6>
                    <p class="h4 text-warning">{{ pendingTasks.length }}</p>
                  </div>
                </div>
                <div class="col-md-3">
                  <div class="text-center">
                    <h6>Progreso</h6>
                    <div class="progress">
                      <div 
                        class="progress-bar bg-success" 
                        :style="{width: progressPercentage + '%'}"
                      >
                        {{ progressPercentage }}%
                      </div>
                    </div>
                  </div>
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
  import TaskForm from '@/components/TaskForm.vue';
  
  export default {
    name: 'CombinedView',
    components: {
      TaskForm
    },
    data() {
      return {
        localTasks: [],
        apiTasks: [],
        loading: false,
        error: '',
        filter: 'all',
        source: 'all',
        searchTerm: ''
      };
    },
    computed: {
      allTasks() {
        return [...this.localTasks, ...this.apiTasks];
      },
      completedTasks() {
        return this.allTasks.filter(task => task.completed);
      },
      pendingTasks() {
        return this.allTasks.filter(task => !task.completed);
      },
      progressPercentage() {
        if (this.allTasks.length === 0) return 0;
        return Math.round((this.completedTasks.length / this.allTasks.length) * 100);
      },
      filteredTasks() {
        return this.allTasks.filter(task => {
          // Filtro por estado
          if (this.filter === 'pending' && task.completed) return false;
          if (this.filter === 'completed' && !task.completed) return false;
  
          // Filtro por fuente
          if (this.source === 'local' && !task.isLocal) return false;
          if (this.source === 'api' && task.isLocal) return false;
  
          // Filtro por búsqueda
          const searchText = (task.title || task.todo || '').toLowerCase();
          if (this.searchTerm && !searchText.includes(this.searchTerm.toLowerCase())) {
            return false;
          }
  
          return true;
        });
      }
    },
    created() {
      this.fetchApiTasks();
    },
    methods: {
      async fetchApiTasks() {
        this.loading = true;
        try {
          const response = await axios.get('https://dummyjson.com/todos');
          this.apiTasks = response.data.todos.map(task => ({
            ...task,
            isLocal: false
          }));
        } catch (err) {
          this.error = 'Error al cargar las tareas de la API.';
          console.error('Error:', err);
        } finally {
          this.loading = false;
        }
      },
      addTask(task) {
        const newTask = {
          ...task,
          id: Date.now(),
          isLocal: true
        };
        this.localTasks.unshift(newTask);
      },
      toggleTask(task) {
        task.completed = !task.completed;
      },
      deleteTask(task) {
        if (task.isLocal) {
          this.localTasks = this.localTasks.filter(t => t.id !== task.id);
        } else {
          this.apiTasks = this.apiTasks.filter(t => t.id !== task.id);
        }
      }
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
  
  .progress {
    height: 20px;
  }
  
  .form-check-label {
    cursor: pointer;
  }
  
  .badge {
    transition: all 0.2s ease;
  }
  </style>