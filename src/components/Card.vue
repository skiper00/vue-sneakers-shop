<script setup>
import { ref } from 'vue'
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  sizeShoes: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})

const selectedProduct = ref(null);
const dialog = ref(false);

const openDialog = (product) => {
  selectedProduct.value = product;
  dialog.value = true
}

</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
    @click='openDialog({ title, imageUrl, price, sizeShoes })'>
    <img @click.stop v-if="onClickFavorite" :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="Like 1"
      class="absolute top-8 left-8" @click="onClickFavorite" />

    <img :src="imageUrl" alt="Sneaker" />

    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>

      <img @click.stop v-if="onClickAdd" @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
    <VDialog v-model="dialog" max-width="600px">
      <VCard>
        <VImg :src="selectedProduct.imageUrl" height="200px"></VImg>
        <VCardText>
          <p class="text-lg"><strong>Название:</strong> {{ selectedProduct.title }}</p>
          <p class="text-lg"><strong>Цена:</strong> {{ selectedProduct.price }} руб.</p>
          <p class="text-lg"><strong>Размер обуви:</strong> {{ selectedProduct.sizeShoes }}</p>
        </VCardText>
      </VCard>
    </VDialog>
  </div>
</template>
