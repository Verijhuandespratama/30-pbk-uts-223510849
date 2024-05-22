<template>
    <div>
      <h1>List Kunjungan Wisata</h1>
      <div class="add-todo">
        <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Tambahkan tempat wisata">
        <button @click="addTodo">Tambah</button>
      </div>
      <div class="filters">
        <label>
          <input type="checkbox" v-model="showIncomplete"> Tampilkan tempat yang belum di kunjungi
        </label>
      </div>
      <ul class="todos">
        <li v-for="(todo, index) in filteredTodos" :key="index" :class="{ 'completed': todo.completed }">
          <input type="checkbox" v-model="todo.completed">
          <span @click="toggleCompletion(todo)" :class="{ 'completed-text': todo.completed }">{{ todo.text }}</span>
          <button @click="removeTodo(index)">Hapus</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      todos: Array,
    },
    data() {
      return {
        newTodo: '',
        showIncomplete: false,
      };
    },
    computed: {
      filteredTodos() {
        if (this.showIncomplete) {
          return this.todos.filter(todo => !todo.completed);
        } else {
          return this.todos;
        }
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim() !== '') {
          this.$emit('add-todo', this.newTodo);
          this.newTodo = '';
        }
      },
      removeTodo(index) {
        this.$emit('remove-todo', index);
      },
      toggleCompletion(todo) {
        this.$emit('toggle-completion', todo);
      }
    }
  };
  </script>
  
  <style scoped>
  /* Your CSS styles here */
  </style>
  