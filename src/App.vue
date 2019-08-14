<template>
  <div id="app">
    <h1>To do list</h1>
    <div class="todo-form">
      <input v-model="newTodo.text" type="text" placeholder="Write a to do..."/>
      <button @click="postTodo">Add it</button>
    </div>

    <div class="todos-container">
      <Todo @deleted-todo="delTodo($event)" v-bind:todo="todo" v-bind:key="`t-${i}`" v-for="(todo, i) in todos"/>
    </div>
  </div>
</template>

<script>
import Todo from './components/todo.vue'
import axios from 'axios'


const baseURL = 'http://localhost:2222/api/todos/'

export default {
  name: 'app',
  components: {
    Todo
  },
  data(){
    return {
      newTodo: {text : '', isCompleted: false},
      todos: []
    }
  },
  mounted() {
    this.getTodos();
  },
  methods : {
    getTodos() {
      axios.get(baseURL)
        .then(res =>  this.todos = res.data)
        .catch(err => console.log(err))
    },
    postTodo() {
      axios.post(baseURL, this.newTodo)
        .then(res => {
          console.log(res.data);
          this.getTodos();
        })
        .catch(err => console.log(err))
    },
    delTodo(id) {
      let todo = this.todos.find(t => t._id == id);
      console.log(todo);
      axios.delete(baseURL+id)
      .then(res => {
        console.log(res)
        this.getTodos();
      })
      .catch(err => console.log(err))
    }
  }
}
</script>

<style>
body {
  background-color: #2c3e50;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.todo-form {
  width: 400px;
  display: flex;
  flex-direction: column;
}
.todo-form input {
  height: 40px;
  border-radius: 3px;
  margin-bottom: 10px;
}
.todo-form button {
  height: 25px;
}
.todos-container {
  width: 400px;
  margin-top: 50px;
  background-color: white;
  color: #2c3e50;
  border-radius: 3px;
}
</style>
