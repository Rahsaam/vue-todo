<template>
  <div class="mt-20 bg-white p-5 shadow-lg rounded-xl border border-red-500 border-2" :class="{completedd: showCompleted}">
    <h1 class="text-center text-3xl font-bold">{{ $route.query.name }}</h1>
    <div class="flex justify-center items-center">
      <div class="my-10 text-center mx-auto inline-block rounded-md shadow-md" :class="[showDetail ? detail : notDetail]">
        <p class="detail" v-if="showDetail && $route.query.detail">{{ $route.query.detail }}</p>
        <p class="notDetail" v-else>There is no detail for this todo.</p>
      </div>
    </div>
      
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
    const showDetail = ref(false)
      watchEffect(() => {
        showCompleted.value = route.query.completed === 'true';
        showDetail.value = route.query.detail !== undefined && route.query.detail !== null
      });
</script>

<style scoped>
  .completedd{
    border: #22a800 3px solid;
  }
  .detail {
  background-color: yellow;
  padding: 8px;
}

.notDetail {
  background-color: green !important;
  color: #fff;
  padding: 8px;
  border-radius: 8px;
}
</style>