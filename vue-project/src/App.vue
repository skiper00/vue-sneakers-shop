<template>
  <div class="w-4/5 m-auto mt-14 bg-white shadow-xl rounded-md">
    <Header />

    <div class="p-10">
      <h2 class="text-3xl font-bold mb-4">Все кроссовки</h2>
      <CardList />
    </div>
  </div>
</template>

<script setup>
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
</script>

<style></style>
