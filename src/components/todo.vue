<template>
  <div class="todo">
    <input type="text" v-if="edit" v-model="todo.text">
    <h2 v-else :class="{'done': todo.isCompleted}">{{todo.text}}</h2>
    <button @click="$emit('deleted-todo', todo._id)" >Delete</button>
    <button @click="toggleDone(todo)">Done</button>
    <button @click="toggleEdit(todo)">Edit</button>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = 'http://localhost:2222/api/todos/'

export default {
  name: 'todo',
  data(){
    return {
      edit: false
    }
  },
  props: {
    todo: {
      text: String,
      _id: String,
      isCompleted: Boolean
    }
  },
  methods:{
    toggleDone(todo){
      axios.patch(baseURL+todo._id, {isCompleted: !todo.isCompleted}).then(res => {
        console.log(res);
        todo.isCompleted = !todo.isCompleted;
      }).catch(err => console.log(err))
    },
    toggleEdit(todo){
      if(!this.edit){
        this.edit = !this.edit;
      } else if(this.edit){
        axios.patch(baseURL+todo._id, {text: todo.text})
          .then(res => {
             this.edit = !this.edit;
          })
          .catch(err => console.log(err));
      }
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
  .edit{
    border: 1px solid #2c3e50;
  }
  input{
    display: block;
    width: 90%;
    height: 40px;
    margin: 5px auto;
  }
</style>
