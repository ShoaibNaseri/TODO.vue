<script setup>
import { ref } from "vue";
import { uid } from "uid";
import VueCreator from '../components/VueCreator.vue';
import TodoItem from "../components/TodoItem.vue";
import { Icon } from '@iconify/vue';

const todoList = ref([]);
const fetchTodoList = () => {
  const saveTodoList = JSON.parse(localStorage.getItem("todoList"));
  if(saveTodoList) {
    todoList.value = saveTodoList;
  }
};
fetchTodoList();

const setTodoListStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    inEditing: null,
  });
  setTodoListStorage();
};
const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
  setTodoListStorage();
};
const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
  setTodoListStorage()
};
const todoUpdate = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
  setTodoListStorage();
};
const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo)=> todo.id !== todoId);
  setTodoListStorage();
}


</script>

<template>
  <main>
      <h1 class="text-center m-3">Todo Creator</h1>
      <VueCreator @create-todo="createTodo" />
      <div class="container">
        <div class="row">
            <div class="col-md-2">

            </div>
            <div class="col-md-6 text-center">
                  <ul class="todo-list" v-if="todoList.length > 0 ">
            <TodoItem v-for="(todo,index) in todoList" :todo = "todo" :index="index" @toggle-complete="toggleTodoComplete" @edit-todo="toggleEditTodo" @update-todo = "todoUpdate"
            @delete-todo = "deleteTodo" />
          </ul>
          <p class="todo-msg mt-2" v-else >
            <Icon icon="noto-v1:sad-but-relieved-face" color="red" width="30" />
            <span>You Don't have todo anything to do..!</span>
            
          </p>
            </div>
        </div>
      </div>

     
  </main>
</template>

<style lang="scss">
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
</style>
