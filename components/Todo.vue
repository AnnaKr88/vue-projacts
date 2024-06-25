<script setup lang="ts">

  import MyForm from "~/elements/MyForm.vue";
  import MyFilter from "~/elements/MyFilter.vue";
  import MyField from "~/elements/MyField.vue";
  import type {ITodoList} from "~/types/types";

  const todoList: Array<ITodoList> = ref([
    {id: 1, title: 'TS', desc: 'About TS'},
    {id: 2, title: 'JS', desc: 'About JS'},
    {id: 3, title: 'PHP', desc: 'About PHP'},
    {id: 4, title: 'Java', desc: 'About Java'},
  ])

  const addTodo = (e:ITodoList) => {
    const obj = {
      id: Date.now(),
      title: e.title,
      desc: e.desc
    }
    console.log(e)
    todoList.value.push(obj)
  }

  const deleteTodo = (e: ITodoList['id']) => {
    todoList.value = todoList.value.filter(el => e !== el.id)
  }
  // const changeTodo = (e: ITodoList) => {
  //   console.log(e)
  //   console.log(todoList.value)
  // }

</script>

<template>
  <div class="todo">
    <MyForm
        @new-todo="addTodo"/>
    <MyFilter />
    <MyField
        :data="todoList"
        @delete-el="deleteTodo"
    />
  </div>
</template>

<style scoped>
 .todo{
   @apply m-3;
 }
</style>