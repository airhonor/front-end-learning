<template>
  <!--  <div id="app">-->
  <!--&lt;!&ndash;  assets 放入静态资源  &ndash;&gt;-->
  <!--    <img alt="Vue logo" src="./assets/logo.png">-->
  <!--    <TodoList/>-->
  <!--    <HelloWorld msg="Welcome to Your Vue.js App" test-text="123545"/>-->
  <!--  </div>-->
  <div id="root">
    <div class="todo-container">
      <h1>待完成事项</h1>
      <a href="https://www.baidu.com">百度</a>
      <div class="todo-wrap">
        <TodoAdder @addTodoItem="addTodoItem"/>
        <TodoList :todoLists="todoLists"/>
        <TodoListInfo :todoLists="todoLists" @clearAllDone="clearAllDone" @selectAllTodo="selectAllTodo"/>
      </div>
    </div>
  </div>
</template>

<script>
import TodoAdder from "@/components/TodoAdder";
import TodoList from "@/components/TodoList";
import TodoListInfo from "@/components/TodoListInfo";

export default {
  name: 'App',
  components: {
    TodoAdder,
    TodoList,
    TodoListInfo
  },
  data() {
    return {
      todoLists: JSON.parse(localStorage.getItem("todoList")) || [],
    }
  },
  methods: {
    deleteTodoItem(id) {
      this.todoLists = this.todoLists.filter(todo => todo.id !== id);
    },
    addTodoItem(todoObj) {
      this.todoLists.unshift(todoObj);
    },
    changeCheckedStatus(id) {
      return this.todoLists.forEach((todo) => {
        if (todo.id == id) {
          todo.done = !todo.done
        }
      })
    },
    //全选or取消全选
    selectAllTodo(done) {
      this.todoLists.forEach((todo) => {
        todo.done = done
      })
    },
    clearAllDone() {
      if (confirm("确定清空所有已完成任务？")) {
        this.todoLists = this.todoLists.filter(todo => {
          return !todo.done;
        })
      }
    },
    updateTodoName(id, nameValue) {
      console.log(id, nameValue);
      this.todoLists.forEach((todo) => {
        if (todo.id == id) {
          todo.todoName = nameValue;
        }
      })
    }
  },
  watch: {
    todoLists: {
      deep: true,
      handler(value) {
        localStorage.setItem('todoList', JSON.stringify(value));
      }
    }
  },
  mounted() {
    this.$bus.$on('changeCheckedStatus', this.changeCheckedStatus);
    this.$bus.$on('deleteTodoItem', this.deleteTodoItem);
    this.$bus.$on('updateTodoName', this.updateTodoName);
  },
  beforeDestroy() {
    this.$bus.$off('changeCheckedStatus')
    this.$bus.$off('deleteTodoItem')
    this.$bus.$off('updateTodoName')
  }
}
</script>

<style>
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
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid #192525;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  color: #fff;
  background-color: #c01455;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
  text-align: center;
}

.todo-container h1 {
  text-align: center;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
