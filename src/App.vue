<script setup lang="ts">
import { nanoid } from 'nanoid'
import { computed, ref } from 'vue'
import ListItem, { type Item } from '@/components/ListItem.vue'
import { formatNumber } from '@/utils'

const items = ref<Item[]>([])
const excludedItems = ref<string[]>([])
const name = ref('')
const cost = ref('0')

const onSubmit = () => {
  items.value.push({
    id: nanoid(),
    name: name.value,
    cost: parseInt(cost.value, 10)
  })
  name.value = ''
  cost.value = '0'
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
    <form @submit.prevent="onSubmit">
      <input type="text" v-model="name" />
      <input type="text" v-model="cost" />
      <button type="submit">Add</button>
    </form>

    <ul role="list">
      <ListItem
        v-for="item in items"
        :key="item.id"
        :item="item"
        :excluded="isExcluded(item.id)"
        :on-remove="onRemove"
        :toggle-exclude="toggleExclude"
      />
    </ul>

    <div>
      Total
      <span>{{ formatNumber.format(total) }}</span>
    </div>
  </main>
</template>

<style scoped>
main {
  padding: var(--size-fluid-5);
}
</style>
