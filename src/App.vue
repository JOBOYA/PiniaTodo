<template>
  <main>

    <!-- Header -->
    <header>
      <img src="./assets/OIP__1_-removebg-preview.svg" alt="pinialogo">
      <h1>Pinia tasks</h1>
    </header>

    <!-- New TaskForm -->

    <div class="new-task-form">
      <TaskForm />
    </div>
     

    <!-- Filter -->

    <nav class="filter">
      <button @click="filter = 'all'">Tâches</button>
      <button @click="filter = 'favorites'">Vos favoris</button>
</nav>

<!-- loading -->
<div class="loading" v-if="loading">
  Loading tasks...
</div>

          <!--Task list-->
      <div class="task-list" v-if="filter === 'all'">
        <p>Vous avez {{ totalCount }} favorites restantes</p>
        <div v-for="task in taskStore.tasks" :key="task.id">
          <TaskDetails :task="task" />

       
        </div>
      </div>

        <div class="task-list" v-if="filter === 'favorites'">
          <p>Vous avez {{ favCount }} tâches restantes</p>
          <div v-for="task in favs" :key="task.id">
            
           
            
          <TaskDetails :task="task" />
          </div>
        </div>
      
        
        <button class="reset"  @click="taskStore.$reset">reset</button>
       

  </main>
</template>

<script>
import { ref } from 'vue'
import TaskDetails from './components/TaskDetails.vue'
import { useTaskStore } from './stores/TaskStore'
import TaskForm from './components/TaskForm.vue'
import { storeToRefs } from 'pinia'


  export default {
    components:{ TaskDetails, TaskForm},
    setup() {
      const taskStore = useTaskStore()

      const {tasks, loading, favs, totalCount, favCount} = storeToRefs(taskStore)
      //fetch tasks
      taskStore.getTasks()

      const filter = ref('')


  
      return {taskStore, filter, tasks, loading, favs, totalCount, favCount}
    }
  }
</script>