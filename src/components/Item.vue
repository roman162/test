<template>
  <div 
    class="item"
    :class="{ selected: selected }"
    :style="{ cursor: clickable ? 'pointer' : 'default' }"
    @click="handleClick"
  >
    {{ item.name }}
  </div>
</template>

<script setup lang="ts">
import type { IItem } from '../types/item'

const props = defineProps<{
  item: IItem
  selected?: boolean
  clickable?: boolean
}>()

const emit = defineEmits<{
  click: [item: IItem]
}>()

const handleClick = (): void => {
  if (props.clickable) {
    emit('click', props.item)
  }
}
</script>

<style scoped lang="scss">
.item {
  padding: 10px 15px;
  border: 2px solid #666;
  border-radius: 4px;
  background-color: #fff;
  transition: all 0.2s;
  user-select: none;
  min-width: 100px;
  text-align: center;

  &:hover {
    background-color: #e8e8e8;
    transform: scale(1.05);
  }

  &.selected {
    background-color: #4CAF50;
    color: white;
    border-color: #45a049;
    font-weight: bold;
  }
}
</style>
