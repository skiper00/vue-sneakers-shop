<template>
    <v-footer class="d-flex align-center justify-center flex-wrap flex-grow-1 py-3" color="#141414">
            <div class="flex-col">
    <v-btn
      v-for="link in ['HOME', 'ABOUTE US', 'TEAM', 'SERVICES', 'BLOG', 'CONTACT US']"
      :key="link"
      :text="link"
      style="font-weight:600 ; color: white;"
      variant="text"
      rounded
    ></v-btn>

    <div class="flex items-center justify-center gap-2">
      <img class="w-11" src="/logo.png" alt=""> — <strong>Vue Sneakers</strong>
    </div>
</div>
  </v-footer>
</template>

<script lang="ts" setup>

</script>

