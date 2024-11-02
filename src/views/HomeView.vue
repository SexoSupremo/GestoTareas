<template>
  <div class="container py-4">
    <!-- Header con logo -->
    <div class="d-flex align-items-center mb-4">
      <img 
        src="@/assets/logo.png" 
        alt="Vue logo" 
        class="me-3"
        style="width: 50px"
      >
      <h1 class="mb-0">Gestor de Tareas</h1>
    </div>

    <!-- Bienvenida -->
    <div class="card mb-4">
      <div class="card-body">
        <h2 class="card-title">Bienvenido a Tareas con Vue</h2>
        <p class="card-text">
          Esta aplicación te permite gestionar tus tareas de manera eficiente y organizada. 
          Puedes crear, actualizar y eliminar tareas, además de obtener tareas desde una API externa.
        </p>
      </div>
    </div>

    <!-- Secciones de la aplicación -->
    <h3 class="mb-3">Secciones de la Aplicación</h3>
    
    <div class="row g-4">
      <!-- Añadir Tarea -->
      <div class="col-md-4">
        <div class="card h-100">
          <div class="card-body">
            <h4 class="card-title">
              <i class="bi bi-plus-circle text-primary me-2"></i>
              Añadir Tarea
            </h4>
            <p class="card-text">
              En esta sección podrás agregar nuevas tareas manualmente. 
              Simplemente ingresa el título de la tarea y será agregada a tu lista personal.
            </p>
            <router-link to="/addtask" class="btn btn-primary">
              Ir a Añadir Tarea
            </router-link>
          </div>
        </div>
      </div>

      <!-- Lista de Tareas -->
      <div class="col-md-4">
        <div class="card h-100">
          <div class="card-body">
            <h4 class="card-title">
              <i class="bi bi-list-check text-success me-2"></i>
              Lista de Tareas
            </h4>
            <p class="card-text">
              Visualiza y gestiona las tareas obtenidas desde la API externa.
              Podrás marcar tareas como completadas y eliminarlas según sea necesario.
            </p>
            <router-link to="/tasklist" class="btn btn-success">
              Ver Lista de Tareas
            </router-link>
          </div>
        </div>
      </div>

      <!-- Vista Combinada -->
      <div class="col-md-4">
        <div class="card h-100">
          <div class="card-body">
            <h4 class="card-title">
              <i class="bi bi-grid-3x3-gap text-info me-2"></i>
              Vista Combinada
            </h4>
            <p class="card-text">
              Esta vista te permite ver todas tus tareas en un solo lugar.
              Combina las tareas locales con las tareas de la API para una gestión integral.
            </p>
            <router-link to="/combined" class="btn btn-info text-white">
              Ver Vista Combinada
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
}
</script>

<style scoped>
.card {
  transition: transform 0.2s ease;
}

.card:hover {
  transform: translateY(-5px);
}

.bi {
  font-size: 1.2rem;
}
</style>
