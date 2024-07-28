<template>
  <div class="border border-slate-200 rounded-xl flex px-5 py-8 items-center gap-4 mt-4">
    <img class="w-16 h-16" :src="imageUrl" alt="Sneaker" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between">
        <b>{{ price }}</b>
        <img
          @click="emit('onClickRemove')"
          class="cursor-pointer transition opacity-40 hover:opacity-100"
          src="/close.svg"
          alt=""
        />
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id:Number,
  title:String,
  imageUrl:String,
  price:Number,
  removeFromCart:Function
})
</script>

<style></style>
