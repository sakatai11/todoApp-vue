<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
import type { CardsProps } from './types/cards'
import TextField from './components/TextField.vue';
import TodoList from './components/TodoList.vue';
const todoCards = ref<CardsProps[]>([])

// ローカルストレージからデータを取得
onMounted(() => {
  const savedTodos = localStorage.getItem('todoCards')
  if (savedTodos) {
    todoCards.value = JSON.parse(savedTodos)
  }
})

// todoCardsが変更されるたびにローカルストレージに保存
watch(todoCards, (newTodos) => {
  localStorage.setItem('todoCards', JSON.stringify(newTodos))
}, { deep: true })


// todo追加
const handleSubmit = (textValue:string) => {
  if (!textValue) {
    return
  } else {
    const newId = todoCards.value.length ? Math.max(...todoCards.value.map(card => card.id)) + 1 : 1
    todoCards.value.push({ id: newId, textValue: textValue, bool: false })
    todoCards.value.sort((a, b) => b.id - a.id)
    console.log('Form submitted:', todoCards.value)
    return true
  }
}

// 完了(トグル)
const toggleBtn = (id: number) => {
  const card = todoCards.value.find((card) => card.id === id)
  if (card) {
    card.bool = !card.bool
  }
  console.log(todoCards.value)
}

// 削除
const deleteBtn = (id: number) => {
  todoCards.value = todoCards.value.filter(card => card.id !== id)
}
</script>

<template>
  <v-app>
    <!-- 内側に記述する -->
    <v-main>
      <v-container max-width="767px">
        <TextField @handleSubmit="handleSubmit" />
        <TodoList @toggle-btn="toggleBtn" @delete-btn="deleteBtn" :todos="todoCards" />
      </v-container>
    </v-main>
    <!-- 内側に記述する -->
  </v-app>
</template>
