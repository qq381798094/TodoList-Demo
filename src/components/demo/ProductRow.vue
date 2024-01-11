<script setup lang="ts">
  defineProps<{
    name: string
    price: string
    stocked: boolean
  }>()
</script>

<template>
  <div :class="stocked ? '' : 'red-font'">
    <span>{{ name }}</span>
    <span class="price">{{ price }}</span>
  </div>
</template>

<style scoped>
  .red-font {
    color: red;
  }
  .price {
    margin-left: 15px;
  }
</style>
