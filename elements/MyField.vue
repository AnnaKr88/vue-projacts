<script setup lang="ts">
  import type {ITodoList} from "~/types/types";
  import MyEl from "~/elements/ui/MyEl.vue";
  interface Props {
    data: ITodoList[]
  }
  const list = defineProps<Props>()

</script>

<template>
  <div class="todo-field">
    <div
        v-if="data.length > 0"
        v-for="(el, index ) in data"
        :key="el.id" >
      <MyEl
        :data="el"
        :index="index"
        @delete-el="$emit('deleteEl', el.id)"
      />
    </div>
    <h1 v-else class="empty">List is empty!</h1>
  </div>
</template>

<style scoped>
  .todo-field{
    @apply grid gap-2 border-2 border-green-800 p-3;


    .empty{
      @apply text-red-950 text-4xl font-extrabold;
    }
  }
</style>