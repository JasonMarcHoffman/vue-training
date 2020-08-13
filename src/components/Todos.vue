<template>
  <div>
    <div v-for="todo in todos" v-bind:key="todo.id">
      <!-- this bind comes from the for loop above -->
      <TodoItem v-bind:todo="todo" v-on:del-todo="$emit('del-todo', todo.id)" />
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'

export default {
  name: 'Todos',
  components: {
    TodoItem,
  },
  // passing the props from the app data
  props: ['todos'],
}
</script>

<style scoped></style>
