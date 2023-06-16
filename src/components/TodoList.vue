<script setup>
import { ref, computed } from "vue";

let id = 0;
const hideCompleted = ref(false);
const title = ref("進階版TodoList");
const newTodo = ref("");
const todos = ref([
  {
    id: id++,
    isGroup: true,
    name: "First",
    data: [
      {
        text: "Hello",
        done: true,
      },
    ],
  },
]);

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

function addGroup() {
  todos.value.push({
    id: id++,
    isGroup: true,
    name: "new Group" + id,
    data: [
      {
        text: newTodo.value,
        done: false,
      },
    ],
  });
}
</script>

<template>
  <h1>{{ title }}</h1>
  <!-- submit.prevent能夠提交且不刷新頁面 -->
  <form @submit.prevent="addTodo">
    <!-- <input v-model="newTodo" /> -->
    <button>Add Todo</button>
  </form>
  <form @submit.prevent="addGroup">
    <button>Add Group</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />

      <input v-model="todo.data[0].text" :class="{ done: todo.done }" />

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
