<template>
  <div class="todo">
    <v-text-field
      @click:append="addTask"
      append-icon="mdi-plus"
      class="pa-3"
      clearable
      hide-details
      @keyup.enter="addTask"
      label="Add Task"
      outlined
      v-model="newTaskTitle"
    >
    </v-text-field>
     <v-list class="pt-0" flat v-if="$store.state.tasks.length">
       <div
        v-for="task in $store.state.tasks" 
        :key="task.id"
       >
        <v-list-item 
          @click="$store.commit('doneTask', task.id)"
          :class="{ 'blue lighten-4' : task.done }"  
        >
          <template v-slot:default>
            <v-list-item-action>
              <v-checkbox :input-value="task.done"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through' : task.done }"
              >
                {{ task.title }}
              </v-list-item-title>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click="$store.commit('deleteTask', task.id)">
                <v-icon color="grey">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </template>
        </v-list-item>
        <v-divider></v-divider>
       </div>
    </v-list>
    <div v-else class="no-tasks">
      <v-icon color="primary" size="100px">mdi-check</v-icon>
      <div class="text-h5 primary--text">No Task</div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Todo',
  data() {
    return {
      newTaskTitle: '',
    }
  },
  methods: {
    addTask() {
      this.$store.commit('addTask', this.newTaskTitle);
      this.newTaskTitle = ''
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    }
  }
}
</script>

<style lang="sass" scoped>
  .no-tasks
    position: absolute
    top: 50%
    left: 50%
    transform: translate(-50%, -50%)
    opacity: 0.5
</style>
