<script setup>
import TodoCreator from "@/components/TodoCreator.vue";
import { ref } from "vue";
import { uid } from "uid";
import TodoItem from "@/components/TodoItem.vue";

const todoList = ref([]);

const setTodoLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};

const fetchTodos = () => {
  const savedTodos = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodos) {
    todoList.value = savedTodos;
  }
};

fetchTodos();

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });

  setTodoLocalStorage();
};

const toggleTodoComplete = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
  setTodoLocalStorage();
};

const toggleEditTodo = (index) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing;
  if (!todoList.value[index].isEditing) {
    setTodoLocalStorage();
  }
};

const updateTodo = (index, todo) => {
  todoList.value[index].todo = todo;
  setTodoLocalStorage();
};

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
  setTodoLocalStorage();
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul v-if="todoList.length > 0" class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">No Todos</p>
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
