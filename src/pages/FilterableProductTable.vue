<script setup lang="ts">
  import { ref, watchEffect } from 'vue'
  import SearchBar from '@/components/demo/SearchBar.vue'
  import ProductTable from '@/components/demo/ProductTable.vue'

  /** 自定义 v-model 数据 */
  const inputValue = ref('')
  const isChecked = ref(false)

  const categoryList = ref([
    { category: 'Fruits', price: '$1', stocked: true, name: 'Apple' },
    { category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit' },
    { category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit' },
    { category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach' },
    { category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin' },
    { category: 'Vegetables', price: '$1', stocked: true, name: 'Peas' },
  ])

  const filterStockList = ref([])
  const filterContentList = ref([])

  //   用于初始化判断数据是否加载成功
  const isValue = ref(false)

  watchEffect(() => {
    // 判断 input 有值但是数组为空的情况
    // if (inputValue.value != '' && filterContentList.value.length === 0) {
    //   // 没有需要的内容
    //   valueFlag.value = false
    // } else {
    //   valueFlag.value = true
    // }

    if (isChecked.value) {
      isValue.value = true
      // 一：只看库存的情况
      filterStockList.value.splice(0)
      filterContentList.value.push(
        ...categoryList.value.filter((item) => item.stocked === true),
      )
      filterStockList.value.push(
        ...categoryList.value.filter((item) => item.stocked === true),
      )
      //   若 input 有输入的值则模糊显示输入的值
      if (inputValue.value != '') {
        filterContentList.value.splice(0)
        filterContentList.value.push(
          ...filterStockList.value.filter((item) => {
            if (
              item.name.toLowerCase().indexOf(inputValue.value) > -1 &&
              !filterContentList.value.includes(item.name)
            ) {
              return true
            }
          }),
        )
      } else {
        filterContentList.value.splice(0)
        filterContentList.value.push(
          ...categoryList.value.filter((item) => item.stocked === true),
        )
      }
    } else {
      // 二、 不看库存的情况
      filterStockList.value.splice(0)
      filterStockList.value.push(...categoryList.value)
      //   若有输入 input 值
      if (inputValue.value != '') {
        filterContentList.value.splice(0)
        filterContentList.value.push(
          ...filterStockList.value.filter((item) => {
            if (
              item.name.toLowerCase().indexOf(inputValue.value) > -1 &&
              !filterContentList.value.includes(item.name)
            ) {
              return true
            }
          }),
        )
      } else {
        filterContentList.value.splice(0)
        filterContentList.value.push(...categoryList.value)
      }
    }
  })
</script>

<template>
  <div>
    <!-- 搜索栏 -->
    <SearchBar v-model:inputValue="inputValue" v-model:isChecked="isChecked" />
    <!-- 列表 -->
    <ProductTable :categoryList="filterContentList" />
    <!-- <div class="empty" v-show="valueFlag === false">当前库存并无当前商品</div> -->
  </div>
</template>

<style scoped></style>
