<template>
   <div>
      <div class="relative bg-white rounded-xl border p-8 cursor-pointer hover:-translate-y-1 transition hover:shadow-xl">
        <img :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" 
        alt="Like" 
        class="absolute"
        @click="onCLickFavorite"
        />
        <img class="mb-3.5" :src="imageUrl" alt="Sneaker" />
        <p>{{ title }}</p>
        
          <div class="flex justify-between">
            <div class="flex flex-col mt-3.5">
              <span class="text-slate-400">ЦЕНА:</span>
              <b>{{ price }} руб.</b>
            </div>
  
            <div>
              <img @click="onCLickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="" />
            </div>
          </div>
        
      </div>
    </div>
</template>

<script setup>
 defineProps({
    id:Number,
    imageUrl:String,
    title:String,
    price:Number,
    isFavorite:Boolean,
    isAdded:Boolean,
    onCLickFavorite:Function,
    onCLickAdd:Function
})


</script>

<style>

</style>