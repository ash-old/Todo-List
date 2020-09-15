<template lang="html">
  <div class="todo-item">
    <div class="item-list" >
      <input type="checkbox" v-model="isDone" @change="doneEdit">
      <div v-if="!editing" :class="{checked : isDone}" v-on:click="editTodo" class="todo-label">{{name}}</div>
      <input v-else type="text" v-model="name" class="item-edit" v-on:blur="doneEdit" v-on:keyup.enter="doneEdit" v-on:keyup.esc="cancelEdit" v-focus>
    </div>
    <div class="delete" type="button" name="button" v-on:click="removeItem(index)">&times;
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },
  data(){
    return {
      'id': this.todo.id,
      'name': this.todo.name,
      'isDone': this.todo.isDone,
      'editing': this.todo.editing,
      'beforeEditCache': ''
    }
  },
  watch: {
    checkAll: function(){
      // if(this.checkAll){
      //   this.isDone = true;
      // } else {
      //   this.isDone = this.todo.isDone;
      // }
      this.checkAll ? this.isDone = true : this.isDone = this.todo.isDone
    }
  },

    directives: {
      focus: {
        inserted: function (el) {
          el.focus()
        }
      }
    },
    
  methods: {
    removeItem(index){
      this.$emit('removedItem', index)
    },
    editTodo: function(){
      this.beforeEditCache = this.name
      this.editing = true
    },
    cancelEdit: function(){
      this.name = this.beforeEditCache
      this.editing = false
    },
    doneEdit: function(){
      if(this.name.trim() == ''){
      // return this.beforeEditCache
      // todo.editing = false
      this.title = this.beforeEditCache
      }
      this.editing = false
      this.$emit('finishedEdit', {
        'index': this.index,
        'todo': {
          'id': this.id,
          'name': this.name,
          'isDone': this.isDone,
          'editing': this.editing
        }
      })
    },
  }
}
</script>

<style lang="css" scoped>

/* .todo-item{
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
  margin-top: 10px;
  padding: 5px;
}

.item-list{
  display: flex;
  justify-content: center;
}

.delete {
  cursor: pointer;
  margin-left: 14px;
  &:hover{
    color: black;
  }
}

.checked{
  text-decoration: line-through;
  color: grey;
} */
</style>
