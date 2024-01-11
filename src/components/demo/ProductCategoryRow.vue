<script setup lang="ts">
  defineProps<{
    title: string
  }>()
</script>

<template>
  <div class="product-category-row-container">{{ title }}</div>
</template>

<style scoped>
  .product-category-row-container {
    font-size: 18px;
    font-weight: bold;
  }
</style>
