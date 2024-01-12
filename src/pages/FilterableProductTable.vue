<script setup lang="ts">
  import { ref, computed } from 'vue'
  import SearchBar from '@/components/demo/SearchBar.vue'
  import ProductTable from '@/components/demo/ProductTable.vue'
  import { type CategoryListItem } from '@/types/product'
  /** 自定义 v-model 数据 */
  /*
  状态提升
  单向数据流
  */
  const inputValue = ref('')
  function setInputValue($val) {
    console.log('父组件提供的set方法触发')
    inputValue.value = $val
  }
  const isChecked = ref(false)

  const categoryList: CategoryListItem[] = [
    { category: 'Fruits', price: '$1', stocked: true, name: 'Apple' },
    { category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach' },
    { category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit' },
    { category: 'Ball', price: '$1', stocked: true, name: 'xxoo' },
    { category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin' },
    { category: 'Ball', price: '$1', stocked: true, name: 'cxk' },
    { category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit' },
    { category: 'Vegetables', price: '$1', stocked: true, name: 'Peas' },
  ]

  const filterList = computed(() => {
    // 浅拷贝原始数据
    let list = categoryList

    return pipeFilterStocked(pipeFilterSearchText(pipeFilterSort(list)))
  })

  // 策略1
  function pipeFilterSearchText(list: CategoryListItem[]) {
    if (inputValue.value.length) {
      return list.filter((item) =>
        item.name
          .toLocaleLowerCase()
          .includes(inputValue.value.toLocaleLowerCase()),
      )
    } else {
      return list
    }
  }

  // 策略2
  function pipeFilterStocked(list: CategoryListItem[]) {
    if (isChecked.value) {
      return list.filter((item) => item.stocked)
    } else {
      return list
    }
  }

  /*
    函数式编程思想：纯函数
    函数具有幂等性（无论执行多少次，相同的输入，始终能得到相同输出，参考 y=2x）
  */
  function pipeFilterSort(list: CategoryListItem[]) {
    return [...list].sort(
      (a, b) => a.category[0].charCodeAt(0) - b.category[0].charCodeAt(0),
    )
  }
</script>

<template>
  <div>
    <!-- 搜索栏 -->
    <SearchBar
      :inputValue="inputValue"
      @update:input-value="setInputValue"
      v-model:isChecked="isChecked"
    />
    <!-- 列表 -->
    <ProductTable :categoryList="filterList" />
    <div v-if="filterList.length === 0" class="empty">当前库存并无当前商品</div>
  </div>
</template>

<style scoped></style>
