<template>
  <div v-show="total" class="todo-info">
    <label>
      <input v-model="selectAll" type="checkbox"/>
    </label>
    <span>
     <span>已完成{{ doneTotal }} / 全部为{{ total }}</span>
   </span>
    <button class="btn btn-danger" @click="clearAllTodo">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "TodoListInfo",
  props: ["todoLists", "selectAllTodo"],
  methods: {
    clearAllTodo() {
      this.$emit('clearAllDone');
    }
  },
  computed: {
    total() {
      return this.todoLists.length;
    },
    //已完成数
    doneTotal() {
      //此处使用reduce方法做条件统计
      /* const x = this.todos.reduce((pre,current)=>{
        console.log('@',pre,current)
        return pre + (current.done ? 1 : 0)
      },0) */
      //简写
      return this.todoLists.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0)
    },
    selectAll: {
      //全选框是否勾选
      get() {
        return this.doneTotal === this.total && this.total > 0
      },
      //isAll被修改时set被调用
      set(value) {
        this.$emit('selectAllTodo', value);
      }
    }
  }
}
</script>

<style scoped>
.todo-info {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-info label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-info label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-info button {
  float: right;
  margin-top: 5px;
}
</style>