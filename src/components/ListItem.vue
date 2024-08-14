<script setup lang="ts">
import { formatNumber } from '@/utils'

export type Item = {
  id: string
  name: string
  cost: number
}

const { item, excluded, onRemove, toggleExclude } = defineProps<{
  item: Item
  excluded: boolean
  onRemove: (id: string) => void
  toggleExclude: (id: string) => void
}>()

const handleRemove = () => onRemove(item.id)
const handleToggleExclude = () => toggleExclude(item.id)
</script>

<template>
  <li class="item">
    <span class="font-bold">{{ item.name }}</span>
    <span>{{ formatNumber.format(item.cost) }}</span>
    <button aria-label="remove {{ item.name }}" @click="handleRemove">X</button>
    <input type="checkbox" :checked="!excluded" @change="handleToggleExclude" />
  </li>
</template>

<style scoped>
.item {
  display: flex;
  gap: 8px;
}
</style>
