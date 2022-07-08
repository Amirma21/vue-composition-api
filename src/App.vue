<template>
  <div class="app">

    <h1>ToDo App</h1>

    <form @submit.prevent="addTodos()">
      <label>new todo</label>
      <input
          v-model="newTodo"
      />
      <button>Add ToDo</button>
    </form>

    <h4>todo list</h4>
    <h2 class="empty-list"
    v-if="todos.length === 0"
    >
      your list is empty
    </h2>

    <div class="todo"
         v-for="(todo , index) in todos"
         :key="index"
         :class="{completed: todo.done}"
    >
      <p :class='{done : todo.done}' @click="doneTodo(todo)">{{ todo.title }}</p>
      <button @click="removeTodo(index)">remove</button>
    </div>
  </div>


</template>

<script setup>
import {ref} from "vue";

const newTodo = ref("")

const getTodos = JSON.parse(localStorage.getItem("todos"))

const todos = ref(getTodos)


function addTodos() {
  newTodo.value && todos.value.push({title: newTodo.value, done: false})
  newTodo.value = ""
  console.log(todos.value)
  saveTodos()
}

const doneTodo = (todo) => {
  todo.done = !todo.done
  console.log(todo.done)
  saveTodos()
}

const removeTodo = (index) => {
  todos.value.splice(index, 1)
  saveTodos()
}

const saveTodos = ()=> {
  localStorage.setItem("todos" , JSON.stringify(todos.value))
}

</script>

<style lang="scss">
$border: 2px solid rgba($color: white, $alpha: 0.35,);

$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #27292d;
$textColor: white;
$primaryColor: #a0a4d9;
$secondTextColor: #1f2023;
body {
  background-color: $backgroundColor;
  margin: 0;
  padding: 0;
  color: $textColor;

  #app {
    max-width: 600px;

    margin: 0 auto;
    padding: 0 20px;

    h1 {
      text-align: center;
    }

    form {
      display: flex;
      flex-direction: column;
      width: 100%;

      label {
        font-size: 20px;
        font-weight: bold;
        margin-bottom: 0.2rem;
      }

      input, button {
        padding: 0.5rem;
        font-size: 20px;
        outline: none;
        border: $border;
        border-radius: 0.4rem;
        margin: 0.5rem 0;
      }

      input {
        background-color: transparent;
        color: $textColor;
      }

      button {
        cursor: pointer;
        background-color: $primaryColor;
      }


    }

    h4 {
      padding: 0.5rem;
      border-bottom: $border;
      font-size: $size4;
    }

    .empty-list{
      text-align: center;
    }

    .completed{
      border-left: 5px solid green !important;
    }

    .todo {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0.7rem 1rem;
      border-right: $border;
      border-bottom: $border;
      border-top:$border;
      border-radius: $size1;
      margin: 1rem 0;
      border-left: 5px solid red ;

      p {
        font-size: 20px;
        cursor: pointer;
      }

      .done {
        text-decoration:  line-through;
      }

      button {
        background-color: $primaryColor;
        outline: none;
        border: $border;
        cursor: pointer;
        height: 35px;
        padding: 0 1rem;
        border-radius: $size1;
        font-weight: bold;
      }
    }

  }
}
</style>
