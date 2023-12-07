<!-- src/components/Todo.vue -->
<template>
  <div class="todo-item">
    <div>
      <input type="checkbox" v-model="isChecked" @change="toggleCompletion" />
      <span :class="{ completed: isChecked }">{{ todo.text }}</span>
    </div>
    <div>
      <button @click="deleteTodo" class="delete-button">Delete</button>
      <button @click="editTodo" class="edit-button">Edit</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['todo'],
  data() {
    return {
      isChecked: this.todo.completed,
    };
  },
  watch: {
    todo: {
      handler() {
        this.isChecked = this.todo.completed;
      },
      deep: true,
    },
  },
  methods: {
    toggleCompletion() {
      this.$emit('toggle-completion', this.todo.id, !this.isChecked);
    },
    deleteTodo() {
      this.$emit('delete-todo', this.todo.id);
    },
    editTodo() {
      this.$emit('edit-todo', this.todo.id);
    },
  },
};
</script>

<style scoped>


.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.completed {
  text-decoration: line-through;
}

.delete-button,
.edit-button {
  margin-left: 8px;
  padding: 8px 16px; /* Add padding for better appearance */
  border: none;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Add a smooth transition effect */
}

.delete-button:hover {
  background-color: #a882ff; /* Change to your desired hover color */
  color: #fff
}

.edit-button:hover {
  background-color: #a882ff; /* Change to your desired hover color */
  color: #fff
}
</style>
