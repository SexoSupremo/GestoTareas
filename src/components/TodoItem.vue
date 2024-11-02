<template>
    <div class="card mb-3">
      <div class="card-body">
        <div class="d-flex justify-content-between align-items-center">
          <div class="d-flex align-items-center">
            <div class="form-check me-3">
              <input 
                type="checkbox" 
                class="form-check-input" 
                :checked="completed"
                @change="$emit('toggle-completion')"
              >
            </div>
            <h5 class="card-title mb-0" :class="{ 'text-decoration-line-through': completed }">
              {{ title }}
            </h5>
          </div>
          <div class="btn-group">
            <button 
              class="btn btn-danger btn-sm"
              @click="$emit('delTodo')"
            >
              <i class="bi bi-trash"></i>
            </button>
          </div>
        </div>
        <p class="card-text mt-2">
          <span :class="completed ? 'badge bg-success' : 'badge bg-warning'">
            {{ completed ? 'Completada' : 'Pendiente' }}
          </span>
        </p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
      name: 'TodoItem',
      props: {
          title: String,
          completed: Boolean,
      }
  }
  </script>
  
  <style scoped>
  .card {
    transition: transform 0.2s;
  }
  
  .card:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  </style>