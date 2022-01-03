<template>
  <div class="home">
    <div class="container-fluid">
      <div class="row justify-content-center">
        <div class="col-md-5 mt-5">
          <h1 class="text-white">TodoApp with Electron!</h1>
          <div class="card">
            <div class="card-body text-left">
              <div>
                <label><strong>New To Do</strong></label>
                <input
                  type="text"
                  class="form-control"
                  placeholder="add to do.."
                  required
                  v-model="todoInput"
                  @keypress.enter="addTodo"
                />
              </div>
              <div class="mt-3">
                <h3>List</h3>
                <TodoListVue
                  @checkItem="itemChecked"
                  @deleteItem="deleteItem"
                  :todoList="this.sortedTodoList"
                ></TodoListVue>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoListVue from "../components/TodoList.vue";

export default {
  name: "Home",
  components: {
    TodoListVue,
  },
  methods: {
    itemChecked(type, index) {
      const getTask = this.todoList.find((T) => T.id == index);
      getTask.status = type;
      getTask.updatedAt = new Date();
      this.saveTodoList();
    },
    addTodo() {
      if (!this.todoInput.length) return;
      this.todoList.push({
        id: this.todoList.length,
        text: this.todoInput,
        createdAt: new Date(),
        status: 0,
      });
      this.todoInput = "";
      this.saveTodoList();
    },
    deleteItem(index) {
      const getTaskIndex = this.todoList.findIndex((T) => T.id == index);
      this.todoList.splice(getTaskIndex, 1);
      this.saveTodoList();
    },
    saveTodoList() {
      localStorage.setItem("todo", JSON.stringify(this.todoList));
    },
  },
  data: () => {
    return {
      todoInput: "",
      todoList: localStorage.getItem("todo")
        ? JSON.parse(localStorage.getItem("todo"))
        : [],
    };
  },
  computed: {
    sortedTodoList: function () {
      const sortTaskList = [...this.todoList].sort(function (a, b) {
        return +new Date(a.createdAt) < +new Date(b.createdAt);
      });
      return sortTaskList;
    },
  },
};
</script>

<style>
body {
  background-color: rgb(24, 24, 24);
}
@import "~bootstrap/dist/css/bootstrap.css";
@import "~bootstrap-icons/font/bootstrap-icons.css";
</style>
