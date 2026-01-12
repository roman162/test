<template>
  <div class="items-container">
    <Item
      v-for="item in items"
      :key="item.id"
      :item="item"
      :selected="isSelected(item.id)"
      :clickable="clickable"
      @click="handleItemClick"
    />
    <div v-if="items.length === 0" class="empty">
      {{ emptyText }}
    </div>
  </div>
</template>

<script setup lang="ts">
import type { IItem } from '../types/item'
import Item from './Item.vue'

const props = withDefaults(defineProps<{
  items: IItem[]
  selectedItems?: IItem[]
  multiple?: boolean
  maxSelection?: number | null
  emptyText?: string
  clickable?: boolean
}>(), {
  selectedItems: () => [],
  multiple: false,
  maxSelection: null,
  emptyText: 'Нет элементов',
  clickable: true
})

const emit = defineEmits<{
  select: [items: IItem[]]
}>()

const isSelected = (id: number): boolean => {
  if (props.multiple) {
    return props.selectedItems.some(item => item.id === id)
  } else {
    return props.selectedItems.length > 0 && props.selectedItems[0]?.id === id
  }
}

const handleItemClick = (item: IItem): void => {
  if (!props.clickable) return

  if (props.multiple) {
    const isCurrentlySelected = isSelected(item.id)
    
    if (isCurrentlySelected) {
      const newSelection = props.selectedItems.filter(selected => selected.id !== item.id)
      emit('select', newSelection)
    } else {
      let newSelection: IItem[]
      if (props.maxSelection && props.selectedItems.length >= props.maxSelection) {
        newSelection = [...props.selectedItems.slice(1), item]
      } else {
        newSelection = [...props.selectedItems, item]
      }
      emit('select', newSelection)
    }
  } else {
    if (isSelected(item.id)) {
      emit('select', [])
    } else {
      emit('select', [item])
    }
  }
}
</script>

<style scoped lang="scss">
.items-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  min-height: 100px;
  padding: 10px;
  border: 1px dashed #ccc;
  border-radius: 4px;

  .empty {
    color: #999;
    font-style: italic;
    width: 100%;
    text-align: center;
    padding: 20px;
  }
}
</style>
