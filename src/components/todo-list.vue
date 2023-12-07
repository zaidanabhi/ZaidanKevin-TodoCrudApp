<!-- src/components/TodoList.vue -->
<template>
  <div>
    <Todo v-for="todo in todos" :key="todo.id" :todo="todo" @toggle-completion="toggleCompletion" @delete-todo="deleteTodo" @edit-todo="editTodo" class="todo-item" />
  </div>
</template>

<script>
import Todo from './Todo.vue';

export default {
  props: ['todos'],
  components: {
    Todo,
  },
  methods: {
    toggleCompletion(id) {
      this.$emit('toggle-completion', id);
    },
    deleteTodo(id) {
      this.$emit('delete-todo', id);
    },
    editTodo(id) {
      this.$emit('edit-todo', id);
    },
  },
};
</script>

<style scoped>
.todo-item {
  margin-bottom: 10px; /* Adjust the margin as needed */
}

.delete-button {
  margin-left: 8px; /* Adjust the margin as needed */
}
</style>
