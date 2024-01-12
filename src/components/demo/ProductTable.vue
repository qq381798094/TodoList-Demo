<script setup lang="ts">
  import { ref, watch } from 'vue'
  import ProductCategoryRow from './ProductCategoryRow.vue'
  import ProductRow from './ProductRow.vue'
  import { type CategoryListItem } from '@/types/product'

  const props = defineProps<{
    categoryList: CategoryListItem[]
  }>()
</script>

<template>
  <div class="product-table-container">
    <div class="title-box">
      <span>名称</span>
      <span class="price">价格</span>
    </div>

    <div v-for="(item, index) in categoryList" :key="item.name">
      <ProductCategoryRow
        v-if="
          categoryList[index - 1]?.category !== categoryList[index].category
        "
        :title="item.category"
      />
      <ProductRow
        :name="item.name"
        :price="item.price"
        :stocked="item.stocked"
      />
    </div>
  </div>
</template>

<style scoped>
  .title-box {
    font-size: 17px;
    font-weight: bold;
  }
  .price {
    margin-left: 15px;
  }
</style>
