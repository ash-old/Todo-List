<template lang="html">

  <div v-on:submit.prevent class="">
    <input v-model="newTodo" v-on:keyup.enter="addItem" type="text" name="todoList" placeholder="todo item..." class="todo-input">
    <transition-group name="fade" enter-active-class="animate__animated animate__fadeInUp" leave-active-class="animate__animated animate__fadeOutDown">
      <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining" @removedItem="removeItem" @finishedEdit="finishedEdit">
      </todo-item>
    </transition-group>
    <div class="extra-container">
      <div>
        <label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check all</label>
      </div>

        <div>
          <button type="button" :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
          <button type="button" :class="{ active: filter == 'active'}" @click="filter = 'active'">Active</button>
          <button type="button" :class="{ active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
          <span>
            <transition name="fade">
            <button v-if="showCompletedButton" :class="{ active: filter == 'clear'}" @click="clearCompleted">Clear Completed</button>
            </transition>
          </span>

        </div>
      <div class="remaining">{{ remaining }} items left</div>
    </div>

</div>

</template>

<script>
import todoItem from './todoItem'

export default {
  name: 'todo-list',
  components: {
    'todo-item': todoItem
  },
  // props:['todos'],
  data(){
    return{
      newTodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      todos:[
        {
          'id': 1,
          'name': "clean kitchen",
          'isDone': false,
          'editing': false
        },
        {
          'id': 2,
          'name': "make dinner",
          'isDone': false,
          'editing': false
        }
      ]
    }
  },

  computed: {
    remaining: function(){
      return this.todos.filter(todo => !todo.isDone).length
    },
    anyRemaining: function(){
      return this.remaining !=0;
    },
    todosFiltered: function(){
      if(this.filter == 'all'){
        return this.todos
      }else if(this.filter == 'active'){
        return this.todos.filter(todo => !todo.isDone)
      }else if(this.filter == 'completed'){
        return this.todos.filter(todo => todo.isDone)
      }
      return this.todos
    },
    showCompletedButton: function(){
      return this.todos.filter(todo => todo.isDone).length > 0;
    }
  },

  methods: {
    addItem: function(){
      if(this.newTodo.trim().length == 0){
        return
      }
      this.todos.push({
        id: this.idForTodo,
        name: this.newTodo,
        isDone: false,
        editing: false
      })
      this.newTodo = ""
      this.idForTodo++
    },
    removeItem: function(index){
      this.todos.splice(index, 1);
    },
    checkAllTodos: function(){
      this.todos.forEach((todo) => todo.isDone = event.target.checked);
    },
    clearCompleted: function(){
      this.todos = this.todos.filter(todo => !todo.isDone)
    },
    finishedEdit: function(data){
      this.todos.splice(data.index, 1, data.todo)
    }
  }
};






</script>

<style lang="css">

.todo-input {
  width:100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus {
    outline:0;
  }
}

.checked{
  text-decoration: line-through;
  color: grey;
}

.todo-item{
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
  margin-top: 10px;
  padding: 5px;
}

.todo-label{
  padding: 0px;
  border: 1px solid white;
  margin-left: 12px;
}

.item-list{
  display: flex;
  justify-content: center;
}

.item-edit{
  margin-top: 10px;
  padding: 5px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding:10px;
  border: 1px solid #ccc;
}

.delete {
  cursor: pointer;
  margin-left: 14px;
  &:hover{
    color: black;
  }
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}

/* button */
button {
  font-size: 14px;
  background-color: white;
  appearance: none;
}

.active {
  background:lightgreen;
}

/* transition */
.fade-enter-active, .fade-leave-active{
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

</style>
