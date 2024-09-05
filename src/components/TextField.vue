<script setup lang="ts">
import { ref, watch } from 'vue'

const text = ref('')

const emit = defineEmits(['handleSubmit'])

const emitHandle = () => {
  emit('handleSubmit', text.value)
  text.value = '' // 入力フィールドをクリア
}

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

</script>
<template>
  <div class="mt-5">
  <v-form
    class="d-flex justify-center ga-5 pr-4 pl-4 mb-4"
    @submit.prevent="emitHandle"
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
</template>