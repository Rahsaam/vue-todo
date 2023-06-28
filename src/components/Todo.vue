<template>
  <div class="todo flex items-center justify-between" :class="{complete: todo.isDone}">
          <div>
            <RouterLink :to="{name: 'todoDetail', params: {id: todo.id}, query: { name: todo.title, completed: todo.isDone, detail: todo.detail }}" >
              <h1 class="font-bold text-xl cursor-pointer hover:font-extrabold inline-block">{{ todo.title }} </h1>
            </RouterLink>
            <div v-if="todo.detail" class="inline-block">
              <i class="fa-solid fa-chevron-down ml-3" v-if="!showDetail" @click="showDetail = !showDetail"></i>
              <i class="fa-solid fa-chevron-up ml-3" v-else @click="showDetail = !showDetail"></i>
            </div>
            <p v-if="showDetail">{{ snipet }}</p>
          </div>
          <div class="space-x-4">
            <i class="fa-solid fa-trash cursor-pointer hover:text-gray-900" @click="$emit('delete')"></i>
            <i class="check fa-solid fa-check cursor-pointer" :checked="todo.isDone" @click="$emit('complete')"></i>
          </div>
        </div>
        <RouterView/>
</template>

<script setup>
import { computed, ref } from "vue"

const props = defineProps(['todo'])
const snipet = computed(() => {
  return props.todo.detail.substring(0,10) + '...'
})
const showDetail = ref(false)
</script>

<style>
    .todo {
    background: #f2f2f2;
    border-left: #e20202 3px solid;
    padding: 15px 10px;
    margin: 15px 0;
    border-radius: 5px;
  }
  .complete {
    border-left: #22a800 3px solid;
    opacity: 0.5;
  }
  .complete h1 {
    text-decoration: line-through;
  }
  .complete .check {
    color: #22a800;
  }
</style>