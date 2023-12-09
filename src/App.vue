<!-- src/App.vue -->
<template>
  <div>
    <h1>Todo App</h1>
    <h2>Made by Zaidan and Kevin</h2>
    <div class="add-todo-container">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new todo" class="new-todo-input" />
      <button @click="addTodo" class="create-button">Create</button>
    </div>
    <div class="todo-list-container">
      <TodoList :todos="todos" @toggle-completion="toggleCompletion" @delete-todo="deleteTodo" @edit-todo="editTodo" />
    </div>
    <div v-if="editingTodo">
      <h2>Edit Todo</h2>
      <input v-model="editedTodo.text" class="edit-textbox" />
      <button @click="updateTodo" class="update-button">Update</button>
      <button @click="cancelEdit" class="cancel-button">Cancel</button>
    </div>
  </div>
</template>

<script>
import TodoList from './components/todo-list.vue';

export default {
  components: {
    TodoList,
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || [],
      newTodo: '',
      editingTodo: null,
      editedTodo: {
        id: null,
        text: '',
        completed: false,
      },
    };
  },
  watch: {
    todos: {
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      deep: true,
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') return;
      const newId = this.todos.length + 1;
      this.todos.push({ id: newId, text: this.newTodo, completed: false });
      this.newTodo = '';
    },
    toggleCompletion(id) {
      const todo = this.todos.find((t) => t.id === id);
      if (todo) {
        todo.completed = !todo.completed;
      }
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    editTodo(id) {
      this.editingTodo = id;
      const todoToEdit = this.todos.find((todo) => todo.id === id);
      this.editedTodo = { ...todoToEdit };
    },
    updateTodo() {
      const index = this.todos.findIndex((todo) => todo.id === this.editedTodo.id);
      if (index !== -1) {
        this.todos.splice(index, 1, this.editedTodo);
      }
      this.cancelEdit();
    },
    cancelEdit() {
      this.editingTodo = null;
      this.editedTodo = {
        id: null,
        text: '',
        completed: false,
      };
    },
  },
};
</script>

<style>

.create-button:hover,
.update-button:hover,
.cancel-button:hover {
  background-color: #a882ff; /* Change to your desired hover color */
  color: #fff; /* Change to your desired hover text color */
}


.add-todo-container {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.new-todo-input {
  flex: 1;
}

.create-button {
  /* Add styles if needed */
}

.todo-list-container {
  margin-top: 16px;
  margin-bottom: 16px;
  border: 2px solid #ccc;
  padding: 16px;
}

.edit-textbox {
  margin-right: 8px; /* Add space to the right of the textbox */
}

.update-button {
  margin-right: 8px;
  /* Add styles if needed */
}
</style>
