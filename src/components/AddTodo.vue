<template>
  <div>
    <!-- adding a listner to the form when it is submitted -->
    <form @submit="addTodo">
      <!-- v-model is from the data function below -->
      <input type="text" v-model="title" name="title" placeholder="add todo" />
      <input type="submit" value="Submit" class="btn" />
    </form>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";

export default {
  name: "addTodo",
  data() {
    return {
      title: "",
    };
  },
  methods: {
    // we want to create a new object
    // using a package called uuid to generate unique id's
    addTodo(e) {
      // dont want the form to submit to the file so we add:
      e.preventDefault();
      const newTodo = {
        id: uuidv4(),
        title: this.title,
        completed: false,
      };
      // need to send this info up to the parent using $emit
      this.$emit("add-todo", newTodo);
      // clear input fielf after submit
      this.title = "";
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
}
input[type="text"] {
  flex: 10;
  padding: 5px;
}
input[type="submit"] {
  flex: 2;
}
</style>
