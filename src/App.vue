<template>
  <section class="todoapp">
    <TodoHeader @addTodo="addTodo" :list="list" />
    <TodoMain :list="showList" @delTodo="delTodo" />
    <TodoFooter
      :count="count"
      @changeTodos="changeTodos"
      @clearDone="clearDone"
    />
  </section>
</template>

<script>
import { nanoid } from "nanoid";
import "./assets/css/base.css";
import "./assets/css/index.css";
import TodoHeader from "./components/TodoHeader.vue";
import TodoMain from "./components/TodoMain.vue";
import TodoFooter from "./components/TodoFooter.vue";

export default {
  name: "App",
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
  data() {
    return {
      list: JSON.parse(localStorage.getItem("todoList")) || [],
      isSel: "",
    };
  },
  methods: {
    addTodo(val) {
      let todo = { id: nanoid(), name: val, isDone: false };
      this.list.unshift(todo);
    },
    delTodo(val) {
      this.list = this.list.filter((ele) => ele.id != val);
    },
    changeTodos(str) {
      this.isSel = str;
    },
    clearDone() {
      this.list = this.list.filter((item) => !item.isDone);
    },
  },
  computed: {
    count() {
      return this.list.filter((ele) => ele.isDone == true).length;
    },
    showList() {
      if (this.isSel === "yes") {
        return this.list.filter((item) => item.isDone);
      } else if (this.isSel === "no") {
        return this.list.filter((item) => !item.isDone);
      } else {
        return this.list;
      }
    },
  },
  watch: {
    list: {
      deep: true,
      handler() {
        localStorage.setItem("todoList", JSON.stringify(this.list));
      },
    },
  },
};
</script>
