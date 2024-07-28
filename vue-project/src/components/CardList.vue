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
      :onCLickFavorite="() => emit('addToFavorite', item)"
      :onCLickAdd="() => emit('addToCart', item)"
      :isAdded="item.isAdded"
    />
  </div>
</template>

<script setup>
import Card from './Card.vue'
defineProps({
  items: Array
})
const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<style></style>
