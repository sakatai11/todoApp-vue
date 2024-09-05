<script setup lang="ts">
import { ref, watch } from 'vue'

const props = defineProps<{text:string}>()

const emit = defineEmits(['handleSubmit', 'updateText'])
const currentText = ref(props.text)
console.log(currentText)


// 親コンポーネントのtextが変更されたときに現在の状態を更新
watch(() => props.text, (newText) => {
  currentText.value = newText
  emit('updateText', newText)
})

const emitHandle = () => {
  emit('handleSubmit', currentText.value)
  currentText.value = '' // 入力フィールドをクリア
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
      v-model="currentText"
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