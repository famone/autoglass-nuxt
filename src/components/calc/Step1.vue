<template>
  <div>
    <div class="space-y-6 mb-6">
      <h3 class="font-halvar text-lg text-white leading-5">
        Рассчитать стоимость на оклейку кузова
      </h3>
    </div>
    <div class="space-y-6">
      <p class="text-white mb-4">
        Укажите тип ТС
      </p>
      <div class="grid grid-cols-3 gap-2">
        <div
          v-for="(item, idx) in types"
          :key="idx"
          class="glass-border-gradient p-2 text-center space-y-2 cursor-pointer select-none"
          :class="{ active: selectedType === item.title }"
          @click="selectedType = item.title"
        >
          <img
            :src="item.icon"
            alt=""
            class="mx-auto"
          >
          <p class=" text-xs sm:text-sm text-white">
            {{ item.title }}
          </p>
        </div>
      </div>
      <p class="text-white mb-4">
        Пакет оклейки
      </p>
      <div class="flex gap-2 flex-wrap">
        <button
          v-for="(item, idx) in packages"
          :key="idx"
          class="outlined-btn"
          :class="{ 'bg-primary !border-primary': [...selectedElements].sort((a, b) => a - b).join('') === item.elements.join('') }"
          @click="selectedElements = [...item.elements]"
        >
          {{ item.title }}
        </button>
      </div>
      <p class="text-white mb-4">
        Выберите элемент
      </p>
      <div class="flex gap-2 flex-wrap">
        <button
          v-for="(item, idx) in elements"
          :key="idx"
          class="outlined-btn"
          :class="{ 'bg-primary !border-primary': selectedElements.includes(item.id) }"
          @click="handleElement(item.id)"
        >
          {{ item.title }}
        </button>
      </div>
      <button
        class="w-full primary-btn mb-2.5"
        :disabled="!selectedType || !selectedElements.length"
        @click="emit('changeSlide')"
      >
        Следующий шаг
      </button>
      <p class="text-white text-center">
        <span class="text-primary">1</span> из 3
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
const emit = defineEmits(['changeSlide'])

const selectedType = defineModel<null | string>('selectedType')
const selectedElements = defineModel<number[]>('selectedElements', { default: [] })

type Props = {
  types: { title: string, icon: string }[]
  packages: { title: string, elements: number[] }[]
  elements: { title: string, id: number, price: number }[]
}
const { types, packages, elements } = defineProps<Props>()

const handleElement = (id: number) => {
  if (!selectedElements.value.includes(id)) {
    selectedElements.value.push(id)
  }
  else {
    const indexInArr = selectedElements.value.indexOf(id)
    selectedElements.value.splice(indexInArr, 1)
  }
}
</script>

<style scoped>
.active.glass-border-gradient::before{
  background: #F24944 !important;
}
</style>
