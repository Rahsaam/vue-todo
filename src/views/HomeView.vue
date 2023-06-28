<template>
  <div class="loginBox bg-white mt-28 shadow-lg rounded-lg p-6">
    <h1 class="text-center text-xl font-bold">Todo list</h1>

    <filter-nav 
    v-if="todosArr.length" 
    @filterChange="current = $event" 
    :current="current"/>

  <form @submit.prevent="submitTodo">
      <div class="flex flex-col mt-4 w-full">
        <label for="">Add todo:</label>
        <div class="w-full flex">
        <input type="text" class="border-b-2 w-full p-1 outline-none" v-model="todoInput" required>
        <button class="hover:bg-green-800" :disabled="!todoInput" :class="{disabled: !todoInput}">Add</button>
        </div>
      </div>
      <div class="mt-5 flex flex-col">
        <label class="" for="">Add detail:</label>
        <textarea class="shadow-md p-1 rounded-lg outline-none mt-3" @keydown.enter="submitTodo" v-model="todoDetail"></textarea>
      </div>
    <div v-if="todosArr.length">
    <div class="mt-10">

      <todo 
      v-for="(todo, index) in filterTodos" 
      :key="todo.id" 
      :todo="todo" 
      @delete="deleteTodo(index)"
      @complete="toggleIsDone(index)"/>

    </div>
    <hr v-if="todosArr.length" class="mt-10">
    <footer class="text-center">
      <p v-if="activeTodos" class="mt-10 text-xl">{{ activeTodos }} active todos</p>
      <p v-else class="mt-10 text-xl">Nothing todo :)</p>
    </footer>
  </div>
    <div v-else>
      <p class="text-center text-xl mt-10">Add your todos</p>
    </div>
    
  </form >
</div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import todo from '../components/Todo.vue'
import filterNav from '../components/FilteredNav.vue'
const todoInput = ref("")
const todosArr = ref([])
const todoDetail = ref("")

  // constant for filter nav
  const current = ref('all')

  const submitTodo = () => {
    if(todoInput.value === '' || isDuplicate(todoInput.value)){
      return
    }
    todosArr.value.unshift({
      id: Math.random(),
      title: todoInput.value,
      detail: todoDetail.value,
      isDone: false
    })
    
    saveToLocalStorage()
    todoInput.value = ""
    todoDetail.value = ''
  }
  // get items from LS to page
  onMounted(() => {
    if(localStorage.getItem('todos') === null) {
      todosArr.value = []
    } else {
      todosArr.value = JSON.parse(localStorage.getItem('todos'))
    }
  })
  // delete todo
  const deleteTodo = (index) => {
    const removeTodo = todosArr.value[index]
    todosArr.value.splice(index, 1)
    removeFromLocalStorage(removeTodo)
  }

  // toggle isDone
  const toggleIsDone = (index) => {
    const todo = todosArr.value[index]
    todo.isDone = !todo.isDone
    checkTodosFromLocalStorage(todo)
  }

  // comput how many active todos
  const activeTodos = computed(() => 
    todosArr.value.filter((todo) => !todo.isDone).length
  )

  // save to lacal storage
  function saveToLocalStorage() {
    localStorage.setItem('todos', JSON.stringify(todosArr.value))
  }

  // remove from local storage
  function removeFromLocalStorage(todo) {
    const storedTodos = JSON.parse(localStorage.getItem('todos')) || []
    const updateTodos = storedTodos.filter((storedTodo) => storedTodo.title !== todo.title)
    localStorage.setItem('todos', JSON.stringify(updateTodos))
  } 

  // isDone in LS
  function checkTodosFromLocalStorage(todo) {
  const storedTodos = JSON.parse(localStorage.getItem('todos'))
  const checkTodos = storedTodos.map(storedTodo => {
    if (storedTodo.id === todo.id) {
    return {
      ...storedTodo,
      isDone: !storedTodo.isDone
    };
  }
  return storedTodo;
})
  localStorage.setItem('todos', JSON.stringify(checkTodos))
}

// filter nav function
const filterTodos = computed(() => 
current.value === 'completed'
    ? todosArr.value.filter((todo) => todo.isDone)
    : current.value === 'ongoing'
      ? todosArr.value.filter((todo) => !todo.isDone)
      : todosArr.value
)

// prevent duplicate todo
function isDuplicate(todoTitle) {
  return todosArr.value.some((todo) => todo.title === todoTitle)
}
</script>

<style scoped>
  button {
    background: #22a800;
    margin-left: 10px;
    padding: 5px 8px;
    color: #fff;
    border-radius: 10px;
  }
  .disabled {
    background: #e8e8e8;
  }
</style>
