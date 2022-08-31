<template>
  <div class="todo-footer" v-if="all">
    <label>
      <input type="checkbox" :checked="isAll" @change="allChange" />
    </label>
    <span>
      <span>已完成{{ wancheng }}</span>
      / 全部{{ all }}
    </span>
    <button class="btn btn-danger" @click="clearDoneTrue">
      清除已完成任务
    </button>
  </div>
</template>

<script>
export default {
  name: "Footer",
  props: ["todos"],
  computed: {
    all() {
      return this.todos.length;
    },
    //遍历每一项,把done为true的返回出去就是完成的
    wancheng() {
      return this.todos.filter((v) => {
        return v.done === true;
      }).length;
    },
    //当已完成和全部一样的时候,全选按钮设为选中状态
    isAll() {
      return this.wancheng === this.all && this.all > 0;
    },
  },
  methods: {
    allChange(e) {
      // console.log(e.target.checked); //点击全选框时,可以通过e.target.checked拿到全选或者全不选
      //之后通知父组件中的数据全部改成这个状态
      this.$emit("changeAll", e.target.checked);
    },
    clearDoneTrue() {
      this.$emit("clearDoneTrue");
    },
  },
};
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  background-color: rgb(239, 7, 7);
  margin-top: 5px;
}
</style>