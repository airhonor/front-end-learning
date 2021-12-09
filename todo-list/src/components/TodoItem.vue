<template>
  <li>
    <label>
      <input :checked="todo.done" type="checkbox" @change="changeChecked(todo.id)"/>
      <span v-show="!todo.isEdit">{{ todo.todoName }}</span>
      <input
          v-show="todo.isEdit"
          ref="inputTodoName"
          :value="todo.todoName"
          type="text"
          @blur="handleBlur(todo,$event)"
      >
    </label>
    <button class="btn btn-danger" @click="deleteItem(todo.id)">删除</button>
    <button v-show="!todo.isEdit" class="btn btn-edit" @click="editTodoItemName(todo)">编辑</button>
  </li>
</template>

<script>

export default {
  name: "TodoItem",
  props: ["todo"],
  methods: {
    deleteItem(id) {
      if (confirm('确定删除吗？')) {
        //通知App组件将对应的todo对象删除/
        // this.deleteTodoItem(id)
        this.$bus.$emit("deleteTodoItem", id);
      }
    },
    changeChecked(id) {
      //this.changeCheckedStatus(id);
      this.$bus.$emit("changeCheckedStatus", id);
    },
    editTodoItemName(todo) {
      console.log("@@@@@@");
      if (todo.hasOwnProperty.call('isEdit')) {
        todo.isEdit = true;
      } else {
        this.$set(todo, 'isEdit', true);
      }
      this.$nextTick(function () {
        this.$refs.inputTodoName.focus();
      })
    },
    //失去焦点回调（真正执行修改逻辑）
    handleBlur(todo, e) {
      todo.isEdit = false
      if (!e.target.value.trim()) return alert('输入不能为空！')
      this.$bus.$emit('updateTodoName', todo.id, e.target.value)
    }
  }
}
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

li label li input .checkbox {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li label li input .input {
  vertical-align: middle;
  align-content: center;
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
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>