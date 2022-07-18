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
      v-model="task"
    />
  </header>
</template>

<script>
export default {
  data() {
    return {
      task: "",
    };
  },
  methods: {
    add() {
      if (this.task.trim() == "") {
        alert("任务名不能为空");
        this.task = "";
        return;
      }
      this.$emit("addTodo", this.task);
      this.task = "";
    },
  },
  computed: {
    isAll: {
      set(val) {
        this.$parent.list.forEach((item) => (item.isDone = val));
      },
      get() {
        return this.$parent.list.every((item) => item.isDone);
      },
    },
  },
};
</script>
