<template>
  <div class="todo">
    <h2 :class="{'done': todo.done}">{{todo.title}}</h2>
    <button @click="$emit('deleted-todo', todo.id)" >Delete</button>
    <button @click="toggleDone(todo)">DONE</button>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = 'http://localhost:3000/todos/'

export default {
  name: 'todo',
  props: {
    todo: {
      title: String,
      id: Number,
      done: Boolean
    }
  },
  methods:{
    toggleDone(todo){
      axios.patch(baseURL+todo.id, {done: !todo.done}).then(res => {
        console.log(res);
        todo.done = !todo.done;
      }).catch(err => console.log(err))
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .todo {
    padding: 10px;
    border-top: 1px solid #2c3e50;
  }
  .todo h2 {
    margin: 5px 0;
  }
  .done{
    text-decoration-line: line-through;
  }
</style>
