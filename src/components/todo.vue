<template>
  <div class="todo">
    <h2>{{todo.title}}</h2>
    <button @click="$emit('deleted-todo', todo.id)" >Delete</button>
  </div>
</template>

<script>
export default {
  name: 'todo',
  props: {
    todo: {
      title: String,
      id: Number
    },
    delTodo: Function
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
</style>
