<template>
  <h4 class="text-center">Your Balance</h4>
  <h1 class="text-center" id="balance">${{ total }}</h1>
</template>
<script setup>
defineProps({
  total: {
    type: Number,
    required: true
  }
})
</script>
