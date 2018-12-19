<template>
  <v-app>
    <v-container>
      <v-layout
        text-xs-center
        wrap
      >
        <v-flex mb-4>
          <h1 class="display-1 success white--text font-weight-bold mb-3">
            To Do List
          </h1>
        </v-flex>
      </v-layout>

      <v-layout row>
      <v-flex xs12 sm10 offset-sm1>
        <v-card>
          <v-toolbar flat>
            <v-toolbar-title class="headline text-xs-center font-weight-bold">Task</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-toolbar-title class="headline text-xs-center font-weight-bold">Done</v-toolbar-title>
          </v-toolbar>
          <v-list v-if="filteredTasks.length === 0">
            <h3 class="success--text text-xs-center">Nothing to do.</h3>
          </v-list>
  
          <v-list v-else>
            <template v-for="(item, index) in filteredTasks">
              <v-list-tile
                :key="item.action"
                @click=""
              >
                <v-list-tile-content>
                  <v-list-tile-title class="text-capitalize">{{ item.action }}</v-list-tile-title>
                </v-list-tile-content>
                <v-list-tile-action>
                  <v-checkbox
                    :label="`${item.done.toString()}`"
                    v-model="item.done"
                  ></v-checkbox>
                </v-list-tile-action>
              </v-list-tile>
              <v-divider
                v-if="index + 1 < tasks.length"
                :key="index"
              ></v-divider>
            </template>
          </v-list>

          <v-form >
            <div style="display: flex;"> 
              <div style="flex-grow: 1">
                <v-text-field
                  v-model="newItemText"
                  label="New task"
                  outline
                  required
                ></v-text-field>
              </div>
              <div>
                <v-btn
                  color="success"
                  @click="addNewTodo"
                >
                  Add
                </v-btn>
              </div>
            </div>
          </v-form>

          <v-list dark>
            <v-list-tile >
                <v-checkbox
                    label="Hide completed tasks"
                    v-model="hideCompleted"
                  ></v-checkbox>
                <v-list-tile-action>
                  <v-btn
                    color="orange"
                    dark
                    @click="deleteCompleted"
                    class="px-2"
                  >
                    Delete completed
                  </v-btn>
                </v-list-tile-action> 
            </v-list-tile>
          </v-list>
        </v-card>
      </v-flex>
    </v-layout>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      newItemText: '',
      hideCompleted: true,
      tasks: []
    }
  },
  computed: {
    filteredTasks() {
      return this.hideCompleted ? this.tasks.filter(t => !t.done) : this.tasks;
    }
  },
  methods: {
    storeData() {
      localStorage.setItem('todos', JSON.stringify(this.tasks));
    },
    addNewTodo() {
      this.tasks.push({
        action: this.newItemText,
        done: false
      });
      this.storeData();
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter(t => !t.done);
      this.storeData();
    }
  },
  created() {
    let data = localStorage.getItem('todos');
    if (data !== null) {
      this.tasks = JSON.parse(data);
    }
  }
}
</script>



