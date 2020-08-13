<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <!-- Binding our todo's data to the todos component as props -->
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from './components/Todos'
import Header from './components/layout/Header'
import AddTodo from './components/AddTodo'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Todos,
    Header,
    AddTodo,
  },
  data() {
    return {
      todos: [],
    }
  },
  methods: {
    // delete todo takes in the id
    // filter loops through like a for loop but you can provide a condition and return an array based on that condition (we want to show all the todos EXCEPT the one with that id)
    deleteTodo(id) {
      // need to add the id on the end of the item we'd like to delete, remember to use `` backticks!
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(
          (res) => (
            (this.todos = this.todos.filter((todo) => todo.id !== id)), res.data
          )
        )
        .catch((err) => console.log(err))
    },
    addTodo(newTodo) {
      // destructure to pull out values from the newTodo
      const { title, completed } = newTodo
      // adding a post request, add another parameter with the data we are sending
      axios
        .post('https://jsonplaceholder.typicode.com/todos', {
          title: title,
          completed: completed,
        })
        // returns a promise so we need a .then and .catch
        .then((res) => (this.todos = [...this.todos, res.data]))
        .catch((err) => console.log(err))
      // using the spread operator to include all existing todos then add a newTodo
      // this.todos = [...this.todos, newTodo]
    },
  },
  // same as component did mount, fires off when the componnt loads
  // making a request to the JSON Placeholder API
  // using axios (npm i axios) simple http library to make requests
  created() {
    // get call the API
    axios
      .get('https://jsonplaceholder.typicode.com/todos?_limit=5')
      // returns a response: res.data
      // we assisgn the todo list to the data returned
      .then((res) => (this.todos = res.data))
      .catch((err) => console.log(err))
  },
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #444;
}
</style>
