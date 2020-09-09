<template>
  <div id="app" class="container">
    <img alt="todo logo" src="./assets/logo.png" class="logo">
    <todo-list/>
  </div>
</template>

<script>
import todoList from './components/todoList.vue'

export default {
  name: 'App',
  data(){
    return{
      // newTodo: "",

    }
  },
  components: {
    'todo-list': todoList
  }
}
</script>

<style>
.logo{
  justify-content: center;
}

* {
  box-sizing:border-box;
}

.container {
  max-width: 500px;
  margin:  auto;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
  /* font-size: 15px; */
}
</style>
