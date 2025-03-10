<script setup>
import axios from 'axios'
import { ref, computed, inject } from 'vue'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { cart, closeDrawer } = inject('cart')
const useData = inject('useData')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const orderCart = [...cart.value]

    const { data } = await axios.post(`https://8549a5b75fa2dfb5.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice
    })

    cart.value = []
    orderId.value = data.id

    await awaitsendOrderToTelegramm(useData.name,useData.email, orderCart, orderId.value)
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const awaitsendOrderToTelegramm = async (name, email, cart, orderId) => {
  const message =
    `
------ Новый заказ ------
              ⤋
  Имя: ${name}📛
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  Email: ${email}✉️
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  Заказ: ${orderId}✨
  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ${cart.map(item => `-${item.title} за ${item.price}₽`).join('\n-----------------------------------------------------------------------------------------------\n')}
  - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  Общая сумма заказ: ${props.totalPrice}💵
  `;

  const botToken = '7955206326:AAFDaAi3NEnbu3VifYwQmLuB1SrfQRNkdEE'
  const chat_id = '-4639541325'

  try {
    const response = await axios.post(`https://api.telegram.org/bot${botToken}/sendMessage`, {
      chat_id,
      text:message
    });
    console.log('Message sending successfully', response.data)
  } catch (e) {
    console.error('Error sending message to Telegram:', e)
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock v-if="!totalPrice && !orderId" title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ." image-url="/package-icon.png" />
      <InfoBlock v-if="orderId" title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png" />
    </div>

    <div v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>

        <button :disabled="buttonDisabled" @click="createOrder"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer">
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
