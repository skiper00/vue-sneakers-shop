<template>
  <div>
    <Drawer
      v-if="drawerOpen"
      :total-price="totalPice"
      :vat-price="vatPrice"
      @create-order="createOrder"
      :button-disabled="cartButtonDisabled"
    />
    <div class="w-4/5 m-auto mt-14 bg-white shadow-xl rounded-md">
      <Header :total-price="totalPrice" @open-drawer="openDrawer" />
      <div class="p-10">
        <div class="flex justify-between items-center">
          <h2 class="text-3xl font-bold">Все кроссовки</h2>
          <div class="flex gap-4">
            <select
              @change="onChangeSelect"
              class="border rounded-md px-3 py-2 outline-none focus:border-gray-400"
            >
              <option value="name">По названию</option>
              <option value="price">По цене(дешевые)</option>
              <option value="-price">По цене(дорогие)</option>
            </select>
            <div class="relative">
              <img class="absolute top-3 left-3 cursor-text" src="/search.svg" alt="" />
              <input
                @input="onChangeSearchInput"
                class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
                placeholder="Поиск..."
              />
            </div>
          </div>
        </div>
        <div class="mt-10">
          <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch, reactive, provide, computed } from 'vue'
import axios from 'axios'
import Drawer from '@/components/Drawer.vue'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
const items = ref([])
const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartIsEmpty = computed(() => cart.value.length === 0)

const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value)

const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
  console.log(item)
  console.log(cart)
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://8549a5b75fa2dfb5.mokky.dev/orders', {
      items: cart.value,
      totalPice: totalPrice.value
    })
    cart.value = []
    return data
  } catch (e) {
    console.log(e)
  } finally {
    isCreatingOrder.value = false
  }
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorite } = await axios.get('https://b4c1f0803b24a479.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorites = favorite.find((fav) => fav.parentId === item.id)
      if (!favorites) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoritesId: favorites.id
      }
    })
    console.log(items.value)
  } catch (e) {
    console.log(e)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parenetId: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post('https://b4c1f0803b24a479.mokky.dev/favorites', obj)

      item.favoriteId = data.id
      console.log(data)
    } else {
      item.isFavorite = false
      await axios.delete(`https://b4c1f0803b24a479.mokky.dev/favorites/${item.favoriteId}`)
    }
  } catch (e) {
    console.log(e)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get('https://174aa6ce323117d6.mokky.dev/items?', { params })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoritesId: null,
      isAdded: false
    }))
    console.log(data)
  } catch (e) {
    console.log(e)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
  if (cart.value.length === 0) {
    closeDrawer()
  }
})
provide('cart', {
  cart,
  closeDrawer,
  addToCart,
  removeFromCart
})
</script>

<style></style>
