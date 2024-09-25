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
  <tr class="border-b hover:bg-gray-100">
    <td class="px-4 py-2">
      {{ item.name }}
    </td>
    <td class="px-4 py-2">
      {{ formatNumber.format(item.cost) }}
    </td>
    <td class="flex px-4 py-2">
      <button
        @click="handleRemove"
        class="mr-3 rounded border-b-4 border-red-600 bg-red-100 px-1 font-medium text-red-900 hover:bg-red-200 focus-visible:outline focus-visible:outline-2 focus-visible:outline-red-600 active:border-y-2 active:border-t-transparent"
      >
        Remove
      </button>
      <label
        class="cursor-pointer rounded border-b-4 border-gray-600 bg-gray-200 px-1 focus-within:outline focus-within:outline-2 focus-within:outline-gray-600 hover:bg-gray-300"
      >
        Exclude
        <input
          type="checkbox"
          class="accent-indigo-500 outline-none"
          :checked="excluded"
          @change="handleToggleExclude"
        />
      </label>
    </td>
  </tr>
</template>
