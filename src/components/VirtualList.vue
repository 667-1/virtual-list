<template>
  <div class="virtual-box" :style="`height: ${height}`" @scroll.passive="onScroll">
    <div :style="{ height: `${scrollList.length * props.itemHeight}px` }">
      <div
          v-for="(item, index) in scrollList.slice(minCount - 2, minCount + (renderCount - 2))"
          :key="index"
          class="virtual-item"
          :style="{ top: `${props.itemHeight * item.realIndex}px` }"
      >
        <template v-if="splitCol === 1">
          <slot :item="item"></slot>
        </template>

        <template v-else>
          <template v-for="(ite, ind) in item.children" :key="ind">
            <slot :item="ite"></slot>
          </template>
        </template>
      </div>
    </div>
  </div>
</template>

<script setup name="virtual-list">
import { watch, ref, defineProps } from 'vue'
import { cloneDeep } from 'lodash-es'

const props = defineProps({
  list: {
    type: Array,
    default: () => [],
  },
  height: {
    type: String,
    default: '0px',
  },
  itemHeight: {
    type: Number,
    default: 0,
  },
  splitCol: {
    type: Number,
    default: 1,
  },
  renderCount: {
    type: Number,
    default: 10,
  },
})

const scrollList = ref([])

const minCount = ref(2)

function onScroll(ev) {
  let scrollTop = ev.target.scrollTop
  if (scrollTop > 2 * props.itemHeight) {
    minCount.value = Math.ceil(scrollTop / props.itemHeight)
  } else {
    minCount.value = 2
  }
}

function splitList() {
  if (props.splitCol === 1) {
    scrollList.value = props.list.map((item, index) => {
      item.realIndex = index
      return item
    })
  } else {
    const length = Math.ceil(props.list.length / props.splitCol)
    const bfList = cloneDeep(props.list)
    const resultList = []

    for (let i = 0; i < length; i++) {
      const item = {
        realIndex: i,
      }

      item.children = bfList.splice(0, props.splitCol)

      resultList.push(item)
    }

    scrollList.value = resultList

    console.log(resultList)
  }
}

watch(
    () => props.list,
    () => {
      splitList()
    },
    {
      immediate: true,
    }
)

watch(
    () => props.splitCol,
    () => {
      splitList()
    }
)
</script>

<style>
.virtual-box {
  position :relative;
  overflow-y: auto;
}

.virtual-item {
  position: absolute;
  display: flex;
}
</style>
