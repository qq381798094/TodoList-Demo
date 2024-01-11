<script setup lang="ts">
  defineProps<{
    inputValue: string
    isChecked: boolean
  }>()

  defineEmits(['update:inputValue', 'update:isChecked'])
</script>

<template>
  <div class="search-bar-container">
    <div class="input-box">
      <input
        type="text"
        :value="inputValue"
        @input="$emit('update:inputValue', $event.target!.value)"
      />
    </div>
    <div class="check-box">
      <input
        type="checkbox"
        :value="isChecked"
        @change="$emit('update:isChecked', $event.target!.checked)"
      />
      <span>只看库存</span>
    </div>
  </div>
</template>

<style scoped></style>
