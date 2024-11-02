<template>
    <div class="container my-5">
        <div class="card shadow-lg border-0">
            <div class="card-header bg-dark text-white text-center">
                <h3 class="h4 mb-0">Gestor de Tareas</h3>
            </div>
            <div class="card-body p-4">
                <div class="input-group mb-4">
                    <input 
                        v-model="newTask" 
                        @keyup.enter="addTask" 
                        placeholder="Escribe una tarea..." 
                        class="form-control form-control-lg rounded-start" 
                        autofocus
                    />
                    <button @click="addTask" class="btn btn-success btn-lg rounded-end">
                        <i class="bi bi-plus-lg"></i> Añadir
                    </button>
                </div>

                <div v-if="tasks.length > 0" class="list-group">
                    <div 
                        v-for="task in tasks" 
                        :key="task.id" 
                        class="list-group-item list-group-item-action d-flex justify-content-between align-items-center py-3"
                    >
                        <div class="d-flex flex-column">
                            <span :class="{ 'text-decoration-line-through text-muted': task.completed }" class="fs-5">
                                {{ task.todo }}
                            </span>
                            <small :class="task.completed ? 'text-success' : 'text-warning'">
                                {{ task.completed ? 'Completada' : 'Pendiente' }}
                            </small>
                        </div>
                        <div>
                            <button 
                                @click="toggleTaskCompletion(task)" 
                                class="btn btn-outline-primary btn-sm me-2"
                            >
                                <i :class="task.completed ? 'bi bi-check-circle-fill' : 'bi bi-circle'"></i>
                                {{ task.completed ? 'Desmarcar' : 'Completar' }}
                            </button>
                            <button 
                                @click="deleteTask(task)" 
                                class="btn btn-outline-danger btn-sm"
                            >
                                <i class="bi bi-trash-fill"></i> Eliminar
                            </button>
                        </div>
                    </div>
                </div>
                <div v-else class="text-center text-muted mt-4">
                    <i class="bi bi-clipboard-check" style="font-size: 2rem;"></i>
                    <p class="mt-2">No hay tareas en la lista. ¡Añade una nueva!</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "AddTask",
    data() {
        return {
            newTask: "",
            tasks: [],
        };
    },
    methods: {
        addTask() {
            if (this.newTask.trim() === "") return;

            const newTask = {
                todo: this.newTask,
                completed: false,
                id: Date.now(),
            };

            this.tasks.unshift(newTask);
            this.newTask = "";
        },
        deleteTask(task) {
            this.tasks = this.tasks.filter((t) => t.id !== task.id);
        },
        toggleTaskCompletion(task) {
            task.completed = !task.completed;
        },
    },
};
</script>

<style scoped>
.text-decoration-line-through {
    text-decoration: line-through;
}
</style>
