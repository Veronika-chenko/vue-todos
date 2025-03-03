<script setup>
import TodoCreator from "@/components/TodoCreator.vue";
import TodoItem from "@/components/TodoItem.vue";
import { uid } from "uid";
import { ref, watch, computed } from "vue";
import { Icon } from "@iconify/vue";
const todos = ref([]);

watch(
  todos,
  () => {
    setTodoListLocalStorage();
  },
  {
    // track changes deep within todoList array:
    deep: true,
  }
);
// automatically tracks the reactive dependencies:
const todoCompleted = computed(() => {
  return todos.value.every((todo) => todo.isCompleted);
});

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todos.value = savedTodoList;
  }
};

fetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todos.value));
};

function createTodo(todo) {
  todos.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false,
  })
}

function toggleTodoComplete(todoId) {
  todos.value = todos.value.map((todo) =>
    todo.id === todoId ? { ...todo, isCompleted: !todo.isCompleted } : todo
  )
}

function toggleEditTodo(todoId) {
  todos.value = todos.value.map((todo) =>
    todo.id === todoId ? { ...todo, isEditing: !todo.isEditing } : todo
  )
}

function updateTodo(value, todoId) {
  if (value.trim()) {
    todos.value = todos.value.map((todo) => (todo.id === todoId ? { ...todo, todo: value } : todo))
  }
}

function deleteTodo(id) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}
</script>

<template>
  <main>
    <h1>Create Todos</h1>
    <TodoCreator @createTodo="createTodo" />
    <ul class="todo-list" v-if="todos.length > 0">
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @toggle-complete="toggleTodoComplete"
        @toggle-edit="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" width="22px" height="22px" />
      <span>You have no todos to complete! Add one!</span>
    </p>
    <p v-if="todoCompleted && todos.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" width="22px" height="22px" />
      <span>You have completed all your todos!</span>
    </p>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
