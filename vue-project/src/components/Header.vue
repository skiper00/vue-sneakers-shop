<template>
    <header class="flex justify-between items-center border-b border-slate-300 py-11 px-11">
        <div class="flex items-center gap-4">
          <img class="w-10" src="/logo.png" alt="Logo" />
          <div>
            <h1 class="text-xl font-bold">VUE SNEAKERS</h1>
            <p class="text-slate-400">Магазин лучших кроссовок</p>
          </div>
        </div>
  
        <div>
          <ul class="flex items-center gap-10">
            <li @click="emit('openDrawer')" class="flex items-center gap-4 cursor-pointer text-gray-500 transition hover:text-black">
              <img src="/cart.svg" alt="Cart">
              <b>{{ totalPrice }} ₽</b>
            </li>
            <li class="flex items-center gap-4 cursor-pointer text-gray-500 transition hover:text-black">
              <img src="/heart.svg" alt="Heart">
              <b>Закладки</b>
            </li>
            <li class="flex items-center gap-4 cursor-pointer text-gray-500 transition hover:text-black">
              <img src="/profile.svg" alt="Profile">
              <b>Профиль</b>
            </li>
          </ul>
        </div>
    </header>
</template>

<script setup>
// import {inject} from 'vue';
defineProps({
  totalPrice:Number
})
const emit = defineEmits(['openDrawer']);

</script>

<style>

</style>