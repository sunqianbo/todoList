<template>
  <div>
    <li>
      <label>
        <input
          type="checkbox"
          :checked="todoObj.done"
          @change="handelCheck(todoObj.id)"
        />
        <input
          v-if="todoObj.isEdit"
          type="text"
          ref="inputTitle"
          :value="todoObj.name"
          @blur="handelBlur(todoObj, $event)"
        />
        <span v-else>{{ todoObj.name }}</span>
      </label>
      <button v-if="!todoObj.isEdit" class="btn" @click="edit(todoObj)">
        编辑
      </button>
      <button class="btn btn-danger" @click="del(todoObj.id)">删除</button>
    </li>
  </div>
</template>

<script>
export default {
  name: "Item",
  props: ["todoObj"],
  methods: {
    //勾选 点击勾选的时候要把原数据的done改了
    handelCheck(id) {
      this.$bus.$emit("everyCheck", id);
    },
    del(id) {
      this.$bus.$emit("delId", id);
    },
    //编辑
    edit(todoObj) {
      //这个不能判断todoObj.isEdit,这个是判断todoObj身上有没有isEdit这个属性
      if (Object.prototype.hasOwnProperty.call(todoObj, "isEdit")) {
        todoObj.isEdit = true;
      } else {
        this.$set(todoObj, "isEdit", true);
      }
      // this.$refs.inputTitle.focus()此时获取不到焦点是因为,页面还没有渲染完毕,就开始调用这句
      // 代码,那就获取不到焦点.所以要等到模板渲染完毕,在执行这句话,就要用到nextTick函数
      this.$nextTick(() => {
        this.$refs.inputTitle.focus();
      });
    },
    //失去焦点时触发
    handelBlur(val, e) {
      val.isEdit = false;
      if (!e.target.value.trim()) return;
      //trim去掉空格返回的字符串有值布尔值就是真的 取反为真的话就是空字符串！！！！！！！！！！
      this.$bus.$emit("updateTodo", val.id, e.target.value);
    },
  },
};
</script>

<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #8cc265;
}
li:hover button {
  display: block;
}
</style>