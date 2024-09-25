<script setup lang="ts">
import { nanoid } from 'nanoid'
import { computed, ref } from 'vue'
import TableRow, { type Item } from '@/components/TableRow.vue'
import { formatNumber } from '@/utils'

const items = ref<Item[]>([])
const excludedItems = ref<string[]>([])
const name = ref('')
const cost = ref('0')
const errors = ref({ name: '', cost: '' })

const onSubmit = () => {
  const parsedCost = parseInt(cost.value.trim(), 10)
  if (isNaN(parsedCost)) {
    errors.value.cost = 'Cost must be a number.'
  } else {
    items.value.push({
      id: nanoid(),
      name: name.value,
      cost: parseInt(cost.value, 10)
    })
    name.value = ''
    cost.value = '0'
  }
}

const onRemove = (id: string) => {
  items.value = items.value.filter((item) => item.id !== id)
  excludedItems.value = excludedItems.value.filter((item) => item !== id)
}

const toggleExclude = (id: string) => {
  if (excludedItems.value.includes(id)) {
    excludedItems.value = excludedItems.value.filter((excluded) => excluded !== id)
  } else {
    excludedItems.value = excludedItems.value.concat(id)
  }
}

const isExcluded = (id: string) => excludedItems.value.includes(id)

const total = computed(() =>
  items.value
    .filter((item) => !excludedItems.value.includes(item.id))
    .map((item) => item.cost)
    .reduce((sum, current) => sum + current, 0)
)
</script>

<template>
  <main>
    <form
      @submit.prevent="onSubmit"
      class="flex flex-col gap-3 md:flex-row md:flex-wrap md:items-end"
    >
      <label>
        <span class="block">Name</span>
        <input
          required
          class="block w-full rounded border-2 border-indigo-400 px-2 py-1 focus:outline focus:outline-2 focus:outline-indigo-400 md:w-auto"
          type="text"
          v-model="name"
        />
      </label>
      <label>
        <span class="block">Cost</span>
        <input
          required
          class="block w-full rounded border-2 border-indigo-400 px-2 py-1 invalid:border-pink-500 invalid:text-pink-600 focus:outline focus:outline-2 focus:outline-indigo-400 focus:invalid:border-pink-500 focus:invalid:ring-pink-500 md:w-auto"
          type="text"
          v-model="cost"
        />
      </label>
      <button
        type="submit"
        class="rounded border-b-4 border-indigo-400 bg-indigo-200 px-3 py-1 font-semibold text-indigo-950 hover:border-indigo-500 hover:bg-indigo-300 focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-400 active:border-y-2 active:border-b-2 active:border-t-transparent"
      >
        Add
      </button>
    </form>

    <div v-if="items.length > 0">
      <table class="my-5 w-full border text-left md:w-4/5 lg:w-3/5">
        <thead class="border-b bg-gray-200 text-sm uppercase">
          <th class="px-4 py-2">Name</th>
          <th class="px-4 py-2">Cost</th>
          <th class="px-4 py-2">Actions</th>
        </thead>
        <tbody>
          <TableRow
            v-for="item in items"
            :key="item.id"
            :item="item"
            :excluded="isExcluded(item.id)"
            :on-remove="onRemove"
            :toggle-exclude="toggleExclude"
          />
        </tbody>
      </table>

      <div class="text-xl">
        <span class="font-semibold">Total </span>
        <span>{{ formatNumber.format(total) }}</span>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  padding: var(--size-fluid-5);
}
</style>
