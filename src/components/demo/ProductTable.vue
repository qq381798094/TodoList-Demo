<script setup lang="ts">
  import { ref } from 'vue'
  import ProductCategoryRow from './ProductCategoryRow.vue'
  import ProductRow from './ProductRow.vue'

  type CategoryListItem = {
    category: string
    price: string
    stocked: boolean
    name: string
  }
  type CategoryList = CategoryListItem[]

  const props = defineProps<{
    categoryList: CategoryList
  }>()

  const cateTitle = ref<string[]>([])
  cateTitle.value.push(
    ...Array.from(new Set(props.categoryList.map((item) => item.category))),
  )
  console.log(cateTitle.value)
</script>

<template>
  <div class="product-table-container">
    <div class="title-box">
      <span>名称</span>
      <span class="price">价格</span>
    </div>

    <div class="category-box" v-for="title in cateTitle" :key="title">
      <ProductCategoryRow :title="title" />
      <ProductRow
        v-for="item in categoryList"
        v-show="item.category === title"
        :key="item.category"
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
