<script setup>
import { ref, computed } from "vue";

let id = 0;
const hideCompleted = ref(false);
const title = ref("TodoList");
const newTodo = ref("");
const todos = ref([{ id: id++, text: "Hello", done: true }]);

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((t) => !t.done) : todos.value;
});

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false });
  newTodo.value = "";
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
}
</script>

<template>
  <h1>{{ title }}</h1>
  <!-- submit.prevent能夠提交且不刷新頁面 -->
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" />
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? "Show All" : "Hide Completed" }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
