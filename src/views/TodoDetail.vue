<template>
  <div class="mt-20 bg-white p-5 shadow-lg rounded-xl border border-red-500 border-2" :class="{completedd: showCompleted}">
    <h1 class="text-center text-3xl font-bold">{{ $route.query.name }}</h1>
    <p class="mt-5 ">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Voluptatem quasi asperiores consectetur corporis porro provident nostrum ipsam dignissimos saepe maiores fuga dolore, tempore deleniti ea, veritatis aspernatur sed quia accusamus!</p>
    <p class="done text-center mt-5 text-green-500 text-lg" v-if="showCompleted">this is complete</p>
    <p class="ongoing text-center mt-5 text-red-500 text-lg" v-else>this is onGoing</p>
  </div>
</template>

<script setup>
    defineProps(['id'])
    // import { useRoute } from 'vue-router';
    // const route = useRoute()
    // const id = route.params.id
    import { ref, watchEffect } from 'vue';
    import { useRoute } from 'vue-router';
    const route = useRoute()
    const showCompleted = ref(false);

      watchEffect(() => {
        showCompleted.value = route.query.completed === 'true';
      });
</script>

<style scoped>
  .completedd{
    border: #22a800 3px solid;
  }
</style>