<template>
  <div class="app">
    <header>
      <nav>
        <ul>
          <li><a href="#" @click="setActive('todos')">Todos</a></li>
          <li><a href="#" @click="setActive('posts')">Post</a></li>
        </ul>
      </nav>
    </header>

    <div v-if="activeSection === 'todos'">
      <TodoList 
        :todos="todos" 
        @add-todo="addTodo" 
        @remove-todo="removeTodo" 
        @toggle-completion="toggleCompletion" 
      />
    </div>

    <div v-else>
      <h1>Posts</h1>
      <div class="filters">
        <label>
          Filter by user:
          <select v-model="selectedUser" @change="fetchPostsByUser">
            <option value="">--Plih User--</option>
            <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
          </select>
        </label>
      </div>
      <ul class="posts">
        <li v-if="posts.length === 0">Please select a user to see posts.</li>
        <li v-for="post in filteredPosts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
          <small>Author: {{ getAuthorName(post.userId) }}</small>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import TodoList from './TodoList.vue';

export default {
  components: {
    TodoList
  },
  data() {
    return {
      todos: [],
      activeSection: 'todos',
      posts: [],
      users: [],
      selectedUser: ''
    };
  },
  computed: {
    filteredPosts() {
      if (this.selectedUser) {
        return this.posts.filter(post => post.userId === parseInt(this.selectedUser));
      } else {
        return this.posts;
      }
    }
  },
  methods: {
    addTodo(newTodo) {
      this.todos.push({ text: newTodo, completed: false });
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    toggleCompletion(todo) {
      todo.completed = !todo.completed;
    },
    setActive(section) {
      this.activeSection = section;
      if (section === 'posts' && this.users.length === 0) {
        this.fetchUsers();
      }
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(users => {
          this.users = users;
        })
        .catch(error => console.error('Error fetching users:', error));
    },
    fetchPostsByUser() {
      if (this.selectedUser) {
        fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`)
          .then(response => response.json())
          .then(posts => {
            this.posts = posts;
          })
          .catch(error => console.error('Error fetching posts:', error));
      } else {
        this.posts = [];
      }
    },
    getAuthorName(userId) {
      const user = this.users.find(user => user.id === userId);
      return user ? user.name : 'Unknown';
    }
  }
};
</script>

<style scoped>


header {
  margin-bottom: 20px;
  margin-top: 20px;
}

nav{
  background-color: aqua;

}

nav ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  justify-content: space-around;
}

nav ul li {
  margin-right: 20px;
}

nav ul li a {
  text-decoration: none;
  color: #000;
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

.posts ul {
  list-style-type: none;
  padding: 0;
}

.posts li {
  margin-top: 10px;
}

.posts h3 {
  margin: 0;
}

.posts p {
  margin: 5px 0;
}
</style>
