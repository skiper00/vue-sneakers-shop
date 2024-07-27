<template>
  <div>
    <!-- <Drawer /> -->
    <div class="w-4/5 m-auto mt-14 bg-white shadow-xl rounded-md">
      <Header />
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
          <CardList :items="items"  />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch, reactive,provide } from 'vue'
import axios from 'axios'
import Drawer from '@/components/Drawer.vue'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

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
      } else {
        return {
          ...item,
          isFavorite: true,
          favoritesId:favorites.id
        }
      }
    })
    console.log(items.value)
  } catch (e) {
    console.log(e)
  }
}

const addToFavorite = async (item) => {
item.isFavorite = true;
console.log(item)
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
provide('addToFavorite',addToFavorite)
</script>

<style></style>
