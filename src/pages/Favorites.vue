<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://b4c1f0803b24a479.mokky.dev/favorites'
    )
    console.log('Закладки:', data)
    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
 
<div v-if="favorites.length > 0">
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <CardList :items="favorites" is-favorites />
</div>
<div v-else>
<p class="font-bold text-green-500 flex justify-center text-xl">У вас пока нет закладок</p>
</div>

</template>
