<template>
  <div class="box">
    <VirtualList :list="list" height="300px" :item-height="40" :split-col="5">
      <template #default="{ item }">
        <div class="item">
          {{ item.value }}
        </div>
      </template>
    </VirtualList>
  </div>
</template>

<script setup>
import VirtualList from '@/components/VirtualList'

const list = Array.from({ length: 100000 }).map((item, index) => ({
  value: `item - ${index + 1}`,
  index: `${index}`
}))
</script>

<style>
.box {
  width: 600px;
  margin: 0 auto;
}

.item {
  line-height: 40px;
  margin-right: 50px;
}
</style>