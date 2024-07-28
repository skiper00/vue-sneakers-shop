<template>
  <div>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white h-full w-96 fixed right-0 top-0 z-20 p-8">
      <DrawerHead />
      <InfoBlock
        v-if="!totalPrice"
        title="Корзина пуста"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />
      <CartItemList />
      <div v-if="totalPrice" class="flex flex-col gap-4 mt-8">
        <div class="flex gap-2">
          <p>Итого:</p>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>
        <div class="flex gap-2">
          <p>Налог 5%:</p>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>
        <button
          @click="() => emit('createOrder')"
          :disabled="buttonDisabled"
          class="mt-7 bg-lime-500 text-white w-full rounded-xl py-3 cursor-pointer transition disabled:bg-slate-400 hover:bg-lime-600 active:bg-lime-700"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean
})
</script>

<style></style>
