<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="t of transactions" :key="t.id" :class="t.amount < 0 ? 'minus' : 'plus'">
      {{ t.text }} <span>${{ t.amount }}</span
      ><button class="delete-btn" @click="handleClick(t.id)">x</button>
    </li>
  </ul>
</template>
<script setup>
defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['deleteTransaction'])

const handleClick = (id) => {
  emit('deleteTransaction', id)
}
</script>
