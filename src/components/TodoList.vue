<script setup lang="ts">
import type { CardsProps } from '@/types/cards';

defineProps<{ 
  todos: CardsProps[], 
}>()


const emit = defineEmits(['update:toggleBtn','update:deleteBtn','update:editBtn', 'update:text'])

</script>
<template>
  <v-container>
    <h3 class="text-center mb-4">todo</h3>
    <div class="list-box border-sm">
      <v-container>
        <template v-for="{ id, textValue, editBool ,bool } in todos.filter(todo => !todo.bool)" :key="id">
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
                  @click="emit('update:toggleBtn', id)"
                />
              </template>
              <template v-else>
                <v-btn
                  variant="text"
                  color="success"
                  density="compact"
                  icon="mdi-check-circle"
                  @click="emit('update:toggleBtn', id)"
                />
              </template>
              <div class="inputTag" v-if="editBool && id">
                <input
                  :value=textValue
                  variant="underlined"
                  @input="(e) => {
                    const target = e.target as HTMLInputElement;
                    if (target) {
                      emit('update:text', id, target.value);
                    }
                  }"
                  :class="{ 'custom-width': textValue, 'error': !textValue }"
                />
                </div>
              <template v-else>
                <v-card-title class="text-h6">
                  {{ textValue }}
                </v-card-title>
              </template>
            </div>
            <div class="d-flex justify-center ga-5">
              <template v-if="editBool">
                <v-btn color="blue" variant="outlined" type="submit" value="flat" @click="() => textValue ? emit('update:editBtn', id) : null">保存</v-btn>
              </template>
              <template v-else>
                <v-btn color="blue" type="submit" value="flat" @click="emit('update:editBtn', id)">編集</v-btn>
              </template>
              <v-btn color="error" type="submit" @click="emit('update:deleteBtn', id)">削除</v-btn>
            </div>
          </v-card>
        </template>
      </v-container>
    </div>
  </v-container>
</template>
<style scoped>
.list-box {
  height: 600px;
  width: 100%;
  overflow-y: auto;
}

.inputTag {
  padding: 0.5rem 1rem;
  line-height: 1.6;
}

.custom-width {
  width: 100%;
  font-size: 1.25rem;
}

.custom-font-color {
  color: #FFF;
}

.error {
  width: 100%;
  border: 2px solid red;
  background-color: rgb(255, 180, 180);
}
</style>