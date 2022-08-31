<template>
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="utf-8" />
      <title>React App</title>

      <link rel="stylesheet" href="index.css" />
    </head>
    <body>
      <div id="root">
        <div class="todo-container">
          <div class="todo-wrap">
            <Header @addTodoObj="addTodoObj"></Header>
            <List :todos="todos" @delApp="delApp"></List>
            <Footer
              :todos="todos"
              @changeAll="changeAll"
              @clearDoneTrue="clearDoneTrue"
            ></Footer>
          </div>
        </div>
      </div>
    </body>
  </html>
</template>

<script>
import Header from "@/components/Header";
import Footer from "@/components/Footer";
import List from "@/components/List";
export default {
  components: { Header, Footer, List },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
      //读取浏览器中的数据,有个注意点就是一开始没有数据,所以要写[]
      // todos: [
      //   { id: "001", name: "孙悟空", done: false },
      //   { id: "002", name: "猪八戒", done: true },
      //   { id: "003", name: "唐三藏", done: false },
      //   { id: "004", name: "沙悟净", done: true },
      //   { id: "005", name: "白龙马", done: false },
      // ],
    };
  },
  mounted() {
    this.$bus.$on("everyCheck", this.checkApp);
    this.$bus.$on("delId", this.delApp);
    this.$bus.$on("updateTodo", this.edit);
  },
  beforeDestroy() {
    this.$bus.$off("everyCheck");
    this.$bus.$off("delId");
    this.$bus.$off("updateTodo");
  },
  methods: {
    //添加
    addTodoObj(val) {
      this.todos.unshift(val);
    },
    //点击每一项的勾选,把done改了
    checkApp(id) {
      this.todos.forEach((v) => {
        if (v.id === id) v.done = !v.done;
      });
    },
    //删除
    delApp(id) {
      this.todos = this.todos.filter((v) => {
        return v.id != id;
      });
    },
    //全选
    changeAll(val) {
      //全选和全不选的逻辑
      // console.log(val, "111111111111");
      this.todos.forEach((v) => (v.done = val));
    },
    //编辑
    edit(id, val) {
      this.todos.forEach((v) => {
        if (v.id === id) {
          v.name = val;
        }
      });
    },
    //清空已完成
    clearDoneTrue(){
     this.todos =  this.todos.filter(v=>{
        return v.done != true
       })
    }
  },
  //利用监视属性把新添加的数据在浏览器本地存一份
  watch: {
    //如果这么写会出现一个bug就是你勾选一个选项,一刷新勾选项就没有了,因为是因为监听的是todos,done在里面,所以要开始深度监听
    // todos(val) {
    //   localStorage.setItem("todos", JSON.stringify(val));
    // },
    todos: {
      deep: true,
      handler(val) {
        localStorage.setItem("todos", JSON.stringify(val));
      },
    },
  },
};
</script>

<style scoped>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>