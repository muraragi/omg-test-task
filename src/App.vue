<script setup lang="ts">
import { ref, watch } from 'vue'
import { RecycleScroller } from 'vue-virtual-scroller'

const data = ref<{ id: number; list: number[] }[]>([])
const shouldUpdateNumbers = ref(true)
const intervalId = ref<number | undefined>()

for (let i = 0; i < randomIntFromInterval(100, 1000); i++) {
  data.value.push({ id: i, list: [] })

  for (let j = 0; j < randomIntFromInterval(10, 30); j++) {
    data.value[i].list.push(randomIntFromInterval(1, 100000))
  }
}

watch(
  shouldUpdateNumbers,
  (newValue) => {
    if (newValue) {
      intervalId.value = setInterval(() => {
        updateDataValues()
      }, 1000)
    } else {
      clearInterval(intervalId.value)
      intervalId.value = undefined
    }
  },
  { immediate: true }
)

function randomIntFromInterval(min: number, max: number) {
  return Math.floor(Math.random() * (max - min + 1) + min)
}

function updateDataValues() {
  for (let i = 0; i < data.value.length; i++) {
    for (let j = 0; j < data.value[i].list.length; j++) {
      data.value[i].list[j] = randomIntFromInterval(1, 100000)
    }
  }
}
</script>

<template>
  <div class="p-10 flex flex-col gap-4">
    <label class="flex items-center gap-2 w-fit">
      <input
        class="w-6 h-6"
        id="update-numbers-flag"
        type="checkbox"
        v-model="shouldUpdateNumbers"
      />

      Включить автообновление данных
    </label>

    <RecycleScroller
      :item-size="112"
      :items="data"
      class="scroller"
      item-class="flex gap-4"
      page-mode
    >
      <template #default="{ item }">
        <div
          v-for="column in item.list"
          :key="column"
          class="flex justify-center shrink-0 items-center w-24 h-24 rounded-lg text-lg font-semibold border border-white transition-transform duration-300 cursor-pointer hover:scale-[.8]"
        >
          {{ column }}
        </div>
      </template>
    </RecycleScroller>
  </div>
</template>
