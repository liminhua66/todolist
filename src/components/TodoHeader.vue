<template>
  <header class="header">
    <h1>todos</h1>
    <input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAll" />
    <label for="toggle-all"></label>
    <!-- label 可以关联一个表单标签 -->
    <input
      class="new-todo"
      placeholder="输入任务名称-回车确认"
      autofocus
      @keyup.enter="add"
      v-model="title"
    />
  </header>
</template>

<script>
export default {
  data() {
    return {
      title: "",
    };
  },
  props: ["addTodo"],
  methods: {
    add() {
      if (this.title.trim() == "") {
        alert("输入不能为空");
        this.title = "";
        return;
      }
      this.$emit("addTodo", this.title);
      this.title = "";
    },
  },
  computed: {
    isAll: {
      set(checked) {
        this.$parent.list.forEach((item) => (item.isDone = checked));
      },
      get() {
        return this.$parent.list.every((item) => item.isDone);
      },
    },
  },
};
</script>
