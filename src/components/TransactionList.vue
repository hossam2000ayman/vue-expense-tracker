<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="(transaction, index) in transactions"
      :key="index"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }}
      <span>{{ transaction.amount.toFixed(2) }}</span>
      <button class="delete-btn" @click="deleteTransaction">x</button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["transactionDeleted"])
const deleteTransaction = (index) => {
  emit("transactionDeleted", index);
};
</script>
