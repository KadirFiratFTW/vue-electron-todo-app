<template>
  <div>
    <ul class="list-group">
      <li
        :class="`list-group-item d-flex justify-content-between align-items-start list-group-item-${
          todoStatus[todo.status].class
        }`"
        v-for="(todo) in todoList"
        :key="todo.id"
      >
        <div>
          <div>
            <span :class="`badge bg-${todoStatus[todo.status].class}`">{{
              todoStatus[todo.status].text
            }}</span>

            <span
              :class="`ml-1 mr-1 badge bg-${todoStatus[todo.status].class}`"
              v-if="todo.status"
              >Updated At : {{ getDate(todo.updatedAt) }}</span
            >
            <span class="badge bg-dark"
              >Created At : {{ getDate(todo.createdAt) }}</span
            >
          </div>
          <div>
            {{ todo.text }}
          </div>
        </div>

        <div class="d-flex">
          <button
            type="button"
            @click="deleteItem(todo.id)"
            class="btn btn-sm btn-outline-dark mr-1"
          >
            <i class="bi bi-trash"></i>
          </button>
          <div class="status-btns" v-if="!todo.status">
            <button
              type="button"
              @click="checkItem(1, todo.id)"
              class="btn btn-sm btn-outline-success mr-1"
            >
              <i class="bi bi-check-circle"></i>
            </button>
            <button
              type="button"
              @click="checkItem(2, todo.id)"
              class="btn btn-sm btn-outline-danger"
            >
              <i class="bi bi-dash-circle"></i>
            </button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>
    

<script>
export default {
  name: "TodoList",
  props: {
    todoList: [],
  },
  methods: {
    checkItem(type, index) {
      this.$emit("checkItem", type, index);
    },
    deleteItem(index) {
      this.$emit("deleteItem", index);
    },
    getDate(date) {
      return new Date(date).toLocaleDateString("tr", {
        weekday: "short",
        year: "numeric",
        month: "2-digit",
        day: "2-digit",
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
      });
    },
  },
  data: () => {
    return {
      todoStatus: {
        0: { text: "In Progress", class: "primary" },
        1: { text: "Completed", class: "success" },
        2: { text: "Canceled", class: "danger" },
      },
    };
  },
};
</script>