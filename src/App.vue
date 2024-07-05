<script>
import StatusFilter from "./components/StatusFilter.vue";
import TodoItem from "./components/TodoItem.vue";
import { todos } from "./utils/data.js";

export default {
  components: {
    StatusFilter,
    TodoItem,
  },
  data() {
    return {
      todos,
      currentTitle: "",
      status: "all",
    };
  },
  computed: {
    activeTodos() {
      return this.todos.filter((todo) => !todo.completed);
    },
    completedTodos() {
      return this.todos.filter((todo) => todo.completed);
    },
  },
  methods: {
    handleSubmit() {
      const newTodo = {
        id: this.todos.length + 1,
        completed: false,
        title: this.currentTitle,
      };

      this.todos.push(newTodo);
      this.currentTitle = "";
    },
  },
};
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">{{ todos.length }}</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">
        <button
          class="todoapp__toggle-all"
          :class="{
            active:
              activeTodos.length === 0 ||
              activeTodos.length === this.todos.length,
          }"
          @click="
            this.todos = this.todos.map((todo) => ({
              ...todo,
              completed: activeTodos.length === 0 ? false : true,
            }))
          "
        ></button>

        <form @submit.prevent="handleSubmit">
          <input
            type="text"
            class="todoapp__new-todo"
            placeholder="What needs to be done?"
            v-model="currentTitle"
          />
        </form>
      </header>

      <section class="todoapp__main">
        <TodoItem
          v-for="(todo, index) of todos"
          :key="todo.id"
          :todo="todo"
          @updateTodo="todos[index] = $event"
          @deleteTodo="todos.splice(index, 1)"
        />
      </section>

      <footer class="todoapp__footer">
        <span class="todo-count"> {{ activeTodos.length }} items left </span>

        <StatusFilter v-model="status" />

        <button
          class="todoapp__clear-completed"
          @click="this.todos = todos.filter((todo) => !todo.completed)"
        >
          Clear completed
        </button>
      </footer>
    </div>

    <article class="message is-danger message--hidden">
      <div class="message-header">
        <p>Error</p>
        <button class="delete"></button>
      </div>

      <div class="message-body">Unable to add a Todo</div>
    </article>
  </div>
</template>
