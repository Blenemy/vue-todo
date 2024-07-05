<script>
export default {
  name: "TodoItem",
  props: {
    todo: Object,
  },
  emits: ["updateTodo", "deleteTodo"],
  data() {
    return {
      isEditing: false,
      newTitle: this.todo.title,
    };
  },
  methods: {
    toggleTodoStatus() {
      this.$emit("updateTodo", {
        ...this.todo,
        completed: !this.todo.completed,
      });
    },
    edit() {
      this.newTitle = this.todo.title;

      this.isEditing = true;

      this.$nextTick(() => {
        this.$refs["title-field"].focus();
      });
    },
    rename() {
      if (!this.isEditing) {
        return;
      }

      this.isEditing = false;

      if (this.newTitle === this.todo.title) {
        return;
      }

      if (!this.newTitle) {
        this.$emit("deleteTodo");

        return;
      }

      this.$emit("updateTodo", {
        ...this.todo,
        title: this.newTitle,
      });

      this.newTitle = "";
    },
  },
};
</script>

<template>
  <div class="todo" :class="{ completed: todo.completed }">
    <label class="todo__status-label">
      <input
        type="checkbox"
        class="todo__status"
        :checked="todo.completed"
        @change="toggleTodoStatus"
      />
    </label>

    <form v-if="isEditing" @submit.prevent="rename">
      <input
        type="text"
        class="todo__title-field"
        placeholder="Empty todo will be deleted"
        @keyup.esc="isEditing = false"
        ref="title-field"
        v-model.trim="newTitle"
        @blur="rename"
      />
    </form>

    <template v-else>
      <span @dblclick="edit" class="todo__title">{{ todo.title }}</span>

      <button class="todo__remove" @:click="$emit('deleteTodo')">x</button>
    </template>

    <div class="modal overlay" :class="{ 'is-active': false }">
      <div class="modal-background has-background-white-ter"></div>
      <div class="loader"></div>
    </div>
  </div>
</template>
