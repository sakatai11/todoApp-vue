<script setup lang="ts">
import { ref } from 'vue'
import type { CardsProps } from './types/cards'

const text = ref('')
const todoCards = ref<CardsProps[]>([])

const rules = [
  (inputValue: string) => {
    // バリデーションをスキップする条件を追加
    if (inputValue === '') {
      return true
    }
    if (!inputValue) {
      return '項目を入力してください'
    }
    return true
  }
]

const handleSubmit = () => {
  if (!text.value) {
    return
  } else {
    const newId = todoCards.value.length ? todoCards.value[todoCards.value.length - 1].id + 1 : 1
    todoCards.value.push({ id: newId, textValue: text.value, bool: false })
    text.value = '' // 入力フィールドをクリア
    console.log('Form submitted:', todoCards.value)
    return true
  }
}

const toggle = (id: number) => {
  const card = todoCards.value.find((card) => card.id === id)
  if (card) {
    card.bool = !card.bool
  }
}
</script>

<template>
  <v-app>
    <!-- 内側に記述する -->
    <v-main>
      <v-container max-width="767px">
        <div class="mt-5">
          <v-form
            class="d-flex justify-center ga-5 pr-4 pl-4 mb-4"
            @submit.prevent="handleSubmit()"
          >
            <v-text-field
              v-model="text"
              :rules="rules"
              hide-details="auto"
              clearable
              label="入力"
              max-width="400px"
            />
            <v-btn color="success" type="submit">追加</v-btn>
          </v-form>
        </div>

        <v-container>
          <div class="list-box border-sm">
            <v-container>
              <template v-for="{ id, textValue, bool } in todoCards" :key="id">
                <v-card
                  class="mx-auto px-6 py-2 mb-4 d-flex justify-space-between align-center"
                  :color="bool ? 'rgb(225 225 225)' : ''"
                >
                  <div class="d-flex justify-center align-center ga-5">
                    <template v-if="!bool">
                      <v-btn
                        variant="text"
                        color="rgb(225 225 225)"
                        density="compact"
                        icon="mdi-check-circle"
                        @click="toggle(id)"
                      />
                    </template>
                    <template v-else>
                      <v-btn
                        variant="text"
                        color="success"
                        density="compact"
                        icon="mdi-check-circle"
                        @click="toggle(id)"
                      />
                    </template>
                    <v-card-title class="text-h5">
                      {{ textValue }}
                    </v-card-title>
                  </div>
                  <div class="d-flex justify-center ga-5">
                    <v-btn color="error" variant="outlined" type="submit">削除</v-btn>
                  </div>
                </v-card>
              </template>
            </v-container>
          </div>
        </v-container>
      </v-container>
    </v-main>
    <!-- 内側に記述する -->
  </v-app>
</template>
<style scoped>
.list-box {
  height: 600px;
  width: 100%;
  overflow-y: auto;
}
</style>
