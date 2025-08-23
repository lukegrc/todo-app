<template>
  <div id="app">
    <Todos
      :todos="todos"
      @add-todo="addTodo"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";
import Todos from "./components/Todos.vue";

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

@Component({
  components: {
    Todos,
  },
})
export default class App extends Vue {
  todos: Todo[] = [
    {
      id: 1,
      title: "Learn Vue.js",
      completed: false,
    },
    {
      id: 2,
      title: "Build a todo app",
      completed: false,
    },
    {
      id: 3,
      title: "Master Vue components",
      completed: false,
    },
  ];

  addTodo(title: string): void {
    const newTodo: Todo = {
      id: Date.now(),
      title: title,
      completed: false,
    };
    this.todos.push(newTodo);
  }

  toggleTodo(id: number): void {
    const todo = this.todos.find((todo: Todo) => todo.id === id);
    if (todo) {
      todo.completed = !todo.completed;
    }
  }

  deleteTodo(id: number): void {
    this.todos = this.todos.filter((todo: Todo) => todo.id !== id);
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.4em;
}
</style>
