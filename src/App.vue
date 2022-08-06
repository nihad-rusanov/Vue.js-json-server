<template>
  <div class="wrapper">
    <div class="header"><header>Todo List</header></div>
    <div class="inputField">
      <input v-model="userData.name" type="text" />
      <button @click="onSave">ekle</button>
    </div>
    <ul v-for="todo in currenTodo" :key="todo.id" class="todoList">
      <li>{{ todo.name }} <button @click="remove(todo.id)">sil</button></li>
    </ul>
    <div class="footer">
      <span> you have pending {{ currenTodo.length }}</span>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      userData: {
        name: null,
      },
      currenTodo: [],
    };
  },
  methods: {
    onSave() {
      console.log(this.userData.name);
      axios
        .post("http://localhost:3000/todos", this.userData)
        .then(() => console.log("todo eklendi"))
        .catch((err) => console.log(err));

         this.currenTodo.push(this.userData);
    },
    remove(id) {
      axios
        .delete(`http://localhost:3000/todos/${id}`)
        .then(() => console.log("ugurla silindi"));

      this.currenTodo = this.currenTodo.filter((todo) => todo.id != id);
    },
  },
  created() {
    axios
      .get("http://localhost:3000/todos")
      .then((response) => (this.currenTodo = response.data));
    console.log(this.currenTodo);
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.wrapper {
  margin: 120px auto;
  max-width: 400px;
  width: 400px;
  background-color: rebeccapurple;
  border-radius: 3px;
}
.header {
  display: flex;
  justify-content: center;
}
header {
  font-size: 26px;
  font-weight: 600;
  margin-top: 20px;
}
.inputField {
  display: flex;
  margin: 20px 10px;
}
.inputField input {
  border-radius: 3px;
  height: 45px;
  outline: none;
  border: none;
  width: 330px;
}
.inputField button {
  width: 45px;
  margin-left: 5px;
  border-radius: 3px;
  border: none;
  background-color: blue;
  color: white;
}
.todoList li {
  list-style: none;
  background-color: #fff;
  margin: 20px 10px;
  height: 30px;
  border-radius: 3px;
  position: relative;
  font-size: 20px;
  font-weight: 300;
}
.todoList li button {
  position: absolute;
  right: 0;
  height: 30px;
  width: 30px;
  background-color: red;
  border: none;
  color: #fff;
}
.footer {
  display: flex;
  justify-content: space-between;
}
.footer span {
  color: #fff;
}

.footer span {
  margin: 10px;
}
</style>
