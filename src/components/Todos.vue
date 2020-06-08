<template>
  <v-container>
    <v-form @submit.prevent='formSubmit'>
       <v-row class="d.flex">
        <v-text-field
          v-model="title"
          regular
          label="Enter Todo"
          clearable >
        </v-text-field>
        <v-btn class="ma-2" outlined color="indigo" type='submit'>Add Todo</v-btn>
      </v-row>
    </v-form>
    <v-card>
      <v-btn text
             class='float-left ma-2'
             color='indigo' type='button' @click='allDone()'>Clear Completed</v-btn>
      <v-tabs v-model='tab'
              background-color="white"
              color="deep-purple accent-4"
              right  >
        <v-tab key='all'> All </v-tab>
        <v-tab key='ongoing'> Ongoing </v-tab>
        <v-tab key='completed'> Completed </v-tab>

        <v-tabs-items v-model='tab'>
          <v-tab-item key='all'>
            <v-container>
              <v-card flat v-for='todo in Todos' :key='todo.text'>
                  <v-row align='center'>
                    <v-col md='6'>
                      <v-checkbox v-model="todo.done"
                                  :label='todo.text'></v-checkbox>
                    </v-col>
                    <v-col md='6'>
                      <v-icon @click='removeTodo(todo)'
                      class='float-right'>mdi-delete</v-icon>
                    </v-col>
                  </v-row>
              </v-card>
            </v-container>
          </v-tab-item>

          <v-tab-item key='ongoing'>
            <v-container>
              <v-card flat v-for='todo in Todos' :key='todo.text'>
                  <v-row align='center' v-if='!todo.done'>
                    <v-col md='6'>
                      <v-checkbox v-model="todo.done"
                                  :label='todo.text'></v-checkbox>
                    </v-col>
                    <v-col md='6'>
                       <v-icon @click='removeTodo(todo)'
                       class='float-right'>mdi-delete</v-icon>
                    </v-col>
                  </v-row>
              </v-card>
            </v-container>
          </v-tab-item>

          <v-tab-item key='completed'>
             <v-container>
              <v-card flat v-for='todo in Todos' :key='todo.text'>
                  <v-row align='center' v-if='todo.done'>
                    <v-col md='6'>
                      <v-checkbox :success='true'
                                  v-model="todo.done"
                                  :label='todo.text' >
                      </v-checkbox>
                    </v-col>
                    <v-col md='6' >
                      <v-icon @click='removeTodo(todo)'
                              class='float-right'>mdi-delete</v-icon>
                    </v-col>
                  </v-row>
              </v-card>
            </v-container>
          </v-tab-item>
        </v-tabs-items>
      </v-tabs>
    </v-card>
  </v-container>
</template>

<script lang="ts">
export default ({

  data: () => ({
    tab: null,
    Todos: [],
    title: '',
  }),

  created() {
    if (localStorage.getItem('Todo-Data')) {
      this.Todos = JSON.parse(localStorage.getItem('Todo-Data'));
    }
    window.addEventListener('beforeunload', this.handler);
  },

  computed: {
  },

  methods: {
    formSubmit() {
      if (this.title) {
        this.Todos.push({
          text: this.title,
          done: false,
        });
      }
      this.title = '';
    },

    removeTodo(todo) {
      const todoIndex = this.Todos.indexOf(todo);
      this.Todos.splice(todoIndex, 1);
    },

    allDone() {
      const activeTodos = this.Todos.filter((todo) => todo.done === false);
      this.Todos = activeTodos;
    },

    handler() {
      localStorage.setItem('Todo-Data', JSON.stringify(this.Todos));
    },
  },
});
</script>


<style scoped>
.todo-done{
  text-decoration: line-through;
}
.todo-text{
  display: inline;
}
</style>
