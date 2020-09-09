<template lang="html">
  <div class="todo-item">
    <div class="item-list" >
      <input type="checkbox" v-model="todo.isDone" >
      <div v-if="!todo.editing" :class="{checked : todo.isDone}" v-on:click="editTodo(todo)" class="todo-label">{{todo.name}}</div>
      <input v-else type="text" v-model="todo.name" class="item-edit" v-on:blur="doneEdit(todo)" v-on:keyup.enter="doneEdit(todo)" v-on:keyup.esc="cancelEdit(todo)" v-focus>
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
    }
  }
}
</script>

<style lang="css" scoped>

.todo-item{
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
  /* margin-top: 10px;
  padding: 5px; */
}

.item-list{
  display: flex;
  justify-content: center;
}
</style>
