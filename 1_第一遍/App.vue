<template>
  <section class="todoapp">
    <TodoHeader @addTodo="addTodo" />
    <TodoMain :list="showList" @delTodo="delTodo" />
    <TodoFooter :count="count" @changeData="changeData" @clear="clear" />
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
      list: JSON.parse(localStorage.getItem("todolist")) || [],
      isSel: "all",
    };
  },
  methods: {
    addTodo(val) {
      let todo = { id: nanoid(), name: val, isDone: false };
      this.list.unshift(todo);
    },
    delTodo(val) {
      const index = this.list.findIndex((ele) => ele.id == val);
      this.list.splice(index, 1);
      // this.list = this.list.map((item) => item.id !== val);
    },
    changeData(str) {
      this.isSel = str;
    },
    clear() {
      return (this.list = this.list.filter((item) => !item.isDone));
    },
  },
  computed: {
    count() {
      return this.list.filter((item) => !item.isDone).length;
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
        localStorage.setItem("todolist", JSON.stringify(this.list));
      },
    },
  },
};
</script>
