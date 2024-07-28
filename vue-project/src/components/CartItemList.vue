<template>
  <div>
    <div class="flex flex-col flex-1 gap-4">
      <CartItem
        v-for="item in cart"
        :key="item.id"
        :title="item.title"
        :price="item.price"
        :image-url="item.imageUrl"
        @on-click-remove="() => removeFromCart(item)"
      />
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue'
import CartItem from '@/components/CartItem.vue'

const { cart, removeFromCart } = inject('cart');
</script>

<style></style>
