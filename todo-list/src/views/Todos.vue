<template>
  <div>
    <h2>Todo application</h2>
    <router-link to="/">Home</router-link>
    <hr />
    <AddTodo @add-todo="addTodo" />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>
    <hr />
    <Loader v-if="loading" />
    <TodoList
      v-else-if="filteredTodos.length"
      v-bind:todos="filteredTodos"
      @remove-todo="removeTodo"
    />
    <!-- <Audio/> -->
    <p v-else>No todos!</p>
  </div>
</template>


<script>
import TodoList from "@/components/TodoList";
import AddTodo from "@/components/AddTodo";
import Loader from "@/components/Loader";
// import Audio from "@/components/Audio";
export default {
  name: "app",
  data() {
    return {
      todos: [
        // { id: 1, title: "To buy bread", completed: false },
        // { id: 2, title: "To buy milk", completed: false },
        // { id: 3, title: "To buy cheese", completed: false },
      ],
      loading: true,
      filter: "all",
    };
  },
  computed: {
      filteredTodos () {
          if (this.filter === 'all') {
              return this.todos
          } 
          if (this.filter === 'completed') {
              return this.todos.filter(item => item.completed)
          }
          if (this.filter === 'not-completed') {
              return this.todos.filter(item => !item.completed)
          }
          return this.filteredTodos
      }
  },
//   watch: {
//       filter (value) {
//           console.log(value)
//       }
//   },
  components: {
    TodoList,
    AddTodo,
    Loader,
    // Audio
  },
  mounted() {
    //       fetch("https://api.sunrise-sunset.org/json?lat=36.7201600&lng=-4.4203400&formatted=0")
    //   .then((response) => response.json())
    //   .then((data) => console.log(data))
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=4/")
      .then((response) => response.json())
      .then((json) => {
        setTimeout(() => {
          this.todos = json;
          this.loading = false;
        }, 1000);
      });
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter((item) => item.id !== id);
    },

    addTodo(todo) {
      this.todos.push(todo);
    },
  },
};
</script>
