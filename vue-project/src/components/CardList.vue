<template>
  <div class="grid grid-cols-4 gap-10">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isFavorite="item.isFavorite"
      :onClickAdd="onClickAdd"
      :isAdded="false"
      :onCLickFavorite="()=>addToFavorite(item)"
    />
  </div>
</template>

<script setup>
import { inject } from 'vue'
import Card from './Card.vue'
defineProps({
  items: Array
})

const onClickAdd = () => {
  alert('Добавить')
}

const addToFavorite = inject('addToFavorite')
</script>

<style></style>
