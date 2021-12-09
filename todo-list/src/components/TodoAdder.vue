<template>
  <div class="todo-adder">
    <input v-model="todoName" placeholder="请输入任务名称，按回车键确认" type="text" @keyup.enter="add"/>
  </div>
</template>

<script>
import {nanoid} from 'nanoid'

export default {
  name: "TodoAdder",
  data() {
    return {
      //收集用户的输入的todo名称
      todoName: ''
    }
  },
  methods: {
    add() {
      if (!this.todoName.trim()) {
        return alert('输入不允许为空');
      }
      const todoObj = {id: nanoid(), todoName: this.todoName, done: false};
      this.$emit('addTodoItem', todoObj);
      this.todoName = '';
    }
  }
}
</script>

<style scoped>
/*  header*/
.todo-adder input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
  text-align: left;
}

.todo-adder input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>