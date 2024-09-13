<script setup lang="ts">
import { ref, watch } from 'vue'

const inputVal = ref({
  text: '',
  error: ''
})

const emit = defineEmits(['handleSubmit'])

const emitHandle = () => {
  if (!inputVal.value.text) {
    inputVal.value.error = '文字を入力してください';
  } else {
    emit('handleSubmit', inputVal.value.text)
    inputVal.value.text = '' // 入力フィールドをクリア
    inputVal.value.error = '' // エラーメッセージをクリア
  }
}

</script>
<template>
  <div class="mt-5">
  <v-form
    class="d-flex justify-center ga-5 pr-4 pl-4 mb-4"
    @submit.prevent="emitHandle"
  >
    <v-text-field
      v-model="inputVal.text"
      hide-details="auto"
      :error-messages="inputVal.error ? inputVal.error : null"
      label="入力"
      max-width="400px"
    />
    <v-btn color="success" type="submit">追加</v-btn>
  </v-form>
</div>
</template>