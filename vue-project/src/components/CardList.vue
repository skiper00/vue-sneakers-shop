<template>
  <div>
    <div class="grid grid-cols-4 gap-10">
      <Card
        image-url="/sneakers/sneakers-1.jpg"
        title="Мужские Кроссовки Nike Blazer Mid Suede"
        :price="6000"
        :is-added="true"
        :is-favorite="false"
        :on-click-add="onCLickAdd"
        />
      <Card />
      <Card />
    </div>
  </div>
</template>

<script setup>
import Card from '@/components/Card.vue'

const onCLickAdd = () => {
    alert('scl')
}

</script>

<style></style>
