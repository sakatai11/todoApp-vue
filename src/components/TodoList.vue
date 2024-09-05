<script setup lang="ts">
import type { CardsProps } from '@/types/cards';

type EventProps = {
  toggle: (id: number) => void
  deleteBtn: (id: number) => void
  todos: CardsProps[]
}

const props = defineProps<EventProps>()
</script>
<template>
  <v-container>
    <div class="list-box border-sm">
      <v-container>
        <template v-for="{ id, textValue, bool } in todos" :key="id">
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
                  @click="props.toggle(id)"
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
              <v-btn color="error" variant="outlined" type="submit" @click="props.deleteBtn(id)">削除</v-btn>
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
</style>