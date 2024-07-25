<script setup>
import { computed, ref } from 'vue'

const header = ref('Shopping List App')
const items = ref([
  {
    id: 1,
    label: '10 party hats',
    qty: 10,
    purchased: true,
    highPriority: false
  },
  {
    id: 2,
    label: '2 board games',
    qty: 1,
    purchased: true,
    highPriority: false
  },
  {
    id: 3,
    label: '20 cups',
    qty: 9,
    purchased: false,
    highPriority: true
  }
])
const newItem = ref('')
const newQty = ref()
const newHighPriorityItem = ref(false)
const editing = ref(false)
const reverseItems = computed(() => {
  return [...items.value].reverse()
})

const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    qty: newQty.value,
    label: newItem.value,
    purchased: false,
    highPriority: newHighPriorityItem.value
  })
  newItem.value = ''
  newQty.value = ''
  newHighPriorityItem.value = false
}

const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
  newQty.value = ''
  newHighPriorityItem.value = false
}

const doPurchased = (item) => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">Add Item</button>
  </div>

  <form class="add-item-form" @submit.prevent="saveItem" v-if="editing">
    <input type="text" v-model="newItem" placeholder="Add an item" style="width: 400px" />
    <input type="number" v-model="newQty" placeholder="qty" style="width: 60px" />
    <label> <input type="checkbox" v-model="newHighPriorityItem" /> High Priority </label>
    <button class="btn btn-primary" :disabled="newItem.length < 3 || newQty < 1" type="submit">
      Save Item
    </button>
  </form>
  <hr />
  <ul>
    <li
      v-for="item in reverseItems"
      @click="doPurchased(item)"
      :key="item.id"
      :class="{ strikeout: item.purchased, priority: item.highPriority }"
    >
      {{ item.qty }}
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">Nothing to see here!</p>
</template>
