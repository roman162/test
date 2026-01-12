<template>
  <div class="app">
    <h1>Тестовое задание VueJS</h1>
    
    <div class="container">
      <div class="top-section">
        <div class="top-left block">
          <h2>Выбранные вещи пользователя</h2>
          <ItemsList
            :items="selectedUserItems"
            :selected-items="[]"
            :clickable="false"
            empty-text="Нет выбранных вещей"
          />
        </div>

        <div class="top-right block">
          <h2>Выбранная вещь на выбор</h2>
          <ItemsList
            :items="selectedAvailableItem ? [selectedAvailableItem] : []"
            :selected-items="[]"
            :clickable="false"
            empty-text="Нет выбранной вещи"
          />
        </div>
      </div>

      <div class="bottom-section">
        <div class="bottom-left block">
          <h2>Вещи пользователя</h2>
          <ItemsList
            :items="userItems"
            :selected-items="selectedUserItems"
            :multiple="true"
            :max-selection="6"
            @select="handleUserItemsSelect"
          />
        </div>

        <div class="bottom-right block">
          <h2>Вещи на выбор</h2>
          <ItemsList
            :items="availableItems"
            :selected-items="selectedAvailableItem ? [selectedAvailableItem] : []"
            :multiple="false"
            @select="handleAvailableItemsSelect"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { IItem } from './types/item'
import ItemsList from './components/ItemsList.vue'

const userItems = ref<IItem[]>([
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" }
])

const availableItems = ref<IItem[]>([
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" }
])

const selectedUserItems = ref<IItem[]>([])

const selectedAvailableItem = ref<IItem | null>(null)

const handleUserItemsSelect = (items: IItem[]): void => {
  selectedUserItems.value = items
}

const handleAvailableItemsSelect = (items: IItem[]): void => {
  selectedAvailableItem.value = items.length > 0 ? items[0]! : null
}
</script>

<style scoped lang="scss">
.app {
  padding: 20px;
  max-width: 1400px;
  margin: 0 auto;

  h1 {
    text-align: center;
    margin-bottom: 30px;
  }

  .container {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .top-section,
  .bottom-section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;

    @media (max-width: 768px) {
      grid-template-columns: 1fr;
    }
  }

  .block {
    border: 2px solid #333;
    padding: 15px;
    border-radius: 8px;
    background-color: #f9f9f9;

    h2 {
      margin-top: 0;
      margin-bottom: 15px;
      font-size: 18px;
      text-align: center;
    }
  }
}
</style>
