<script setup>
import { defineProps } from 'vue';
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  }
});
const emit = defineEmits(['transactionDeleted']);
const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}

</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="t in transactions" :key="t.id" :class="t.amount < 0 ? 'minus' : 'plus'">
      {{ t.text }}<span>${{ t.amount }}</span>
      <button @click="deleteTransaction(t.id)" class=" delete-btn">x</button>
    </li>
  </ul>
</template>
