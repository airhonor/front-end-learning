<template>
  <div class="todo-list">
    <TodoItem
        v-for="todoObj in todoLists"
        :key="todoObj.id"
        :todo="todoObj"
    />
  </div>
</template>


<script>
import TodoItem from "@/components/TodoItem";

export default {
  name: "TodoList",
  components: {
    TodoItem
  },
  props: ["todoLists"]
}
</script>


<style scoped>
.todo-list {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  text-align: center;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>