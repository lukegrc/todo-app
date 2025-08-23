<template>
  <div class="todos-container">
    <h1>Todo App</h1>

    <!-- Add new todo form -->
    <div class="add-todo">
      <input
        v-model="newTodoTitle"
        @keyup.enter="addTodo"
        placeholder="Add a new todo..."
        class="todo-input"
      />
      <button @click="addTodo" class="add-btn">Add</button>
    </div>

    <!-- Todo list -->
    <div class="todo-list">
      <div
        v-for="todo in todos"
        :key="todo.id"
        class="todo-item"
        :class="{ completed: todo.completed }"
      >
        <div class="todo-content">
          <input
            type="checkbox"
            :checked="todo.completed"
            @change="toggleTodo(todo.id)"
            class="todo-checkbox"
          />
          <span class="todo-title">{{ todo.title }}</span>
        </div>
        <button @click="deleteTodo(todo.id)" class="delete-btn">
          ×
        </button>
      </div>
    </div>

    <!-- Summary -->
    <div class="todo-summary" v-if="todos.length > 0">
      <span>{{ remainingTodos }} of {{ todos.length }} remaining</span>
      <button
        @click="clearCompleted"
        v-if="completedTodos > 0"
        class="clear-btn"
      >
        Clear completed
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";

interface Todo {
  id: number;
  title: string;
  completed: boolean;
}

@Component({
  props: {
    todos: {
      type: Array as () => Todo[],
      required: true,
    },
  },
})
export default class Todos extends Vue {
  // Declare the prop as a class property
  todos!: Todo[];
  newTodoTitle = "";

  get remainingTodos(): number {
    return this.todos.filter((todo: Todo) => !todo.completed).length;
  }

  get completedTodos(): number {
    return this.todos.filter((todo: Todo) => todo.completed).length;
  }

  addTodo(): void {
    if (this.newTodoTitle.trim()) {
      this.$emit("add-todo", this.newTodoTitle.trim());
      this.newTodoTitle = "";
    }
  }

  toggleTodo(id: number): void {
    this.$emit("toggle-todo", id);
  }

  deleteTodo(id: number): void {
    this.$emit("delete-todo", id);
  }

  clearCompleted(): void {
    this.todos.forEach((todo: Todo) => {
      if (todo.completed) {
        this.deleteTodo(todo.id);
      }
    });
  }
}
</script>

<style scoped>
.todos-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 30px;
}

.add-todo {
  display: flex;
  margin-bottom: 20px;
  gap: 10px;
}

.todo-input {
  flex: 1;
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 6px;
  font-size: 16px;
  outline: none;
  transition: border-color 0.3s;
}

.todo-input:focus {
  border-color: #42b983;
}

.add-btn {
  padding: 12px 24px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.add-btn:hover {
  background-color: #3aa876;
}

.todo-list {
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px;
  margin-bottom: 10px;
  background-color: #f8f9fa;
  border-radius: 8px;
  border-left: 4px solid #42b983;
  transition: all 0.3s;
}

.todo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.todo-item.completed {
  border-left-color: #6c757d;
  opacity: 0.7;
}

.todo-content {
  display: flex;
  align-items: center;
  gap: 12px;
  flex: 1;
}

.todo-checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.todo-title {
  font-size: 16px;
  color: #2c3e50;
}

.todo-item.completed .todo-title {
  text-decoration: line-through;
  color: #6c757d;
}

.delete-btn {
  background: none;
  border: none;
  color: #dc3545;
  font-size: 20px;
  cursor: pointer;
  padding: 5px 10px;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.delete-btn:hover {
  background-color: #f8d7da;
}

.todo-summary {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background-color: #e9ecef;
  border-radius: 8px;
  color: #6c757d;
}

.clear-btn {
  background-color: #6c757d;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.clear-btn:hover {
  background-color: #5a6268;
}
</style>
