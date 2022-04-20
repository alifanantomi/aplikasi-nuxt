<template>
  <div id="app" class="py-4">
    <div class="container">

      <div class="title border-bottom d-flex align-items-center py-2">
        <h5>Task</h5>

        <div class="d-flex align-items-center ms-auto">

          <button class="btn btn-outline-primary py-1 px-3 me-4" @click="shuffle">
            Shuffle!
          </button>

          <input type="text" class="form-control" placeholder="Search" v-model="searchQuery">

          <div class="d-flex align-items-center justify-content-end w-100">
            <span class="me-2">View As</span>
            
            <button 
              class="btn btn-outline-secondary py-1 px-3" 
              @click="isGrid = !isGrid"
            >
              {{ isGrid ? 'Grid' : 'List' }}
            </button>
          </div>
        </div>
      </div>

      <transition-group name="task" tag="div" class="list-task row">

        <CardItem 
          v-for="task in tasks" 
          :task="task" 
          :key="task.id" 
          :isGrid="isGrid" 
        />

      </transition-group>

      <div class="action py-2">
        <a href="#" class="add-button" v-if="!isCreating" @click="isCreating = !isCreating">Add Task</a>
        <form class="add-card" v-on:submit.prevent="onSubmit" v-else>
          <div class="card mb-2">
            <div class="card-body d-flex flex-column p-0">
              <input 
                class="form-control border-0 mb-2" 
                placeholder="Title" 
                type="text" 
                v-model="task.title"
                >
              <textarea 
                class="form-control border-0 small" 
                placeholder="Description" 
                rows="3" 
                v-model="task.description"
              ></textarea>
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2" type="submit">Save</button>
            <button class="btn btn-outline-secondary" @click="isCreating = !isCreating">Cancel</button>
          </div>
        </form>
      </div>

    </div>
  </div>
</template>
<script>
import CardItem from "@/components/Card/CardItem.vue"

import { mapState, mapActions, mapMutations, mapGetters } from "vuex"

export default {
  layout: 'app',
  components: {
    CardItem
  },
  data() {
    return {
      searchQuery: '',

      // Status saat menambahkan task
      isCreating: false,

      // Tipe layout daftar task
      isGrid: false, 
      
      // Menampung data form
      task: {
        id: 3,
        title: '',
        description: '',
        isDone: false
      }
    }
  },
  computed: {
    ...mapState('tasks', ['tasks']),
    
    // ...mapGetters({
    //   tasks: 'tasks/getTasks',
    // }),

    resultQuery() {
      if (this.searchQuery) {
        return this.tasks.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(" ")
            .every((v) => item.title.toLowerCase().includes(v));
        });
      } else {
        // console.log(this.tasks);
        return this.tasks
      }

    }
  },

  methods: {
    ...mapActions('tasks', ['addTask']),

    onSubmit() {
      this.task.id += 1

      var payload = this.task

      this.addTask(payload)

      this.task = {
        title: '',
        description: ''
        
      }
    },

    shuffle() {
      this.tasks = _.shuffle(this.tasks)
    }
  }
}
</script>

<style scoped>

#app .task-move {
  transition: transform .4s;
}

</style>