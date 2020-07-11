<template lang="html">
  <div class="">
    <div v-on:submit.prevent class="">
      <input v-model="newTodo" v-on:keyup.enter="addItem" type="text" name="todoList" placeholder="todo item..." class="search-bar">
    </div>
    <div v-for="(todo,index) in todos" :key="todo.name" class="item">
      <div >
        <!-- <div class="item-edit" v-if="!todo.editing" contenteditable="" v-on:keyup.enter="doneEdit(todo)" v-on:keyup.esc="cancelEdit(todo)">{{todo.name}}</div> -->
        <div v-if="!todo.editing" v-on:click="editTodo(todo)">{{todo.name}}</div>
        <input v-else type="text" v-model="todo.name" class="item-edit" v-on:blur="doneEdit(todo)" v-on:keyup.enter="doneEdit(todo)" v-on:keyup.esc="cancelEdit(todo)">
      </div>
      <div class="delete" type="button" name="button" v-on:click="removeItem(index)">&times;</div>

      <!-- <button v-if="!isDone" type="button" name="button" v-on:click="removeItem(index)">Completed!</button> -->
    </div>


  </div>
</template>

<script>
export default {
  name: 'todo-list',
  // props:['todos'],
  data(){
    return{
      newTodo: "",
      beforeCacheEdit: "",
      todos:[
        {
          'name': "clean kitchen",
          'isDone': false,
          'editing': false
        },
        {
          'name': "make dinner",
          'isDone': false,
          'editing': false
        }
      ]
    }
  },
  methods: {
    addItem: function(){
      if(this.newTodo.trim().length == 0){
        return
      }

      this.todos.push({
        name: this.newTodo,
        isDone: false,
        editing: false
      })
      this.newTodo = ""
    },

    editTodo: function(todo){
      this.beforeCacheEdit = todo.name
      todo.editing = true
    },
    cancelEdit: function(todo){
      todo.name = this.beforeCacheEdit
      todo.editing = false
    },

    doneEdit: function(todo){
      todo.editing = false
    },

    removeItem: function(index){
      this.todos.splice(index, 1);
    }
  }
};






</script>

<style lang="css" scoped>


.item{
  margin-top: 10px;
  padding: 5px;
}
.item-edit{
  margin-top: 10px;
  padding: 5px;

}

.delete {
  cursor: pointer;
}
</style>
