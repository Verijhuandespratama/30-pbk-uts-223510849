<template>
  <div class="app">
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
  data() {
    return {
      newTodo: '',
      showIncomplete: false,
      todos: []
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
        this.todos.push({ text: this.newTodo, completed: false });
        this.newTodo = '';
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleCompletion(todo) {
      todo.completed = !todo.completed;
    }
  }
};
</script>

<style scoped>
.app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

.add-todo input {
  padding: 5px;
  margin-right: 10px;
}

.add-todo button {
  padding: 5px 10px;
}

.filters {
  margin-top: 10px;
}

.todos {
  list-style-type: none;
  padding: 0;
}

.todos li {
  margin-top: 5px;
}

.completed {
  text-decoration: line-through;
}

.completed-text {
  padding-left: 5px;
}
</style>
