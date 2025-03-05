<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="income" :expense="expense" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed, watch } from "vue";

import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref(
  localStorage.getItem("transactions")
    ? JSON.parse(localStorage.getItem("transactions"))
    : [
        { text: "Flower", amount: -20 },
        { text: "Salary", amount: 300 },
        { text: "Book", amount: -10 },
        { text: "Camera", amount: 150 },
      ]
);

// Watch for changes and update localStorage automatically
watch(
  transactions,
  (newTransactions) => {
    localStorage.setItem("transactions", JSON.stringify(newTransactions));
  },
  { deep: true } // Ensures it reacts to changes within the array
);

//Get total
const total = computed(() => {
  return transactions.value
    .reduce((accumulator, transaction) => {
      return accumulator + transaction.amount;
    }, 0)
    .toFixed(2);
});

//Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((accumulator, transaction) => accumulator + transaction.amount, 0)
    .toFixed(2);
});
//Get expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((accumulator, transaction) => accumulator + transaction.amount, 0)
    .toFixed(2);
});

const handleTransactionSubmitted = (transaction) => {
  transactions.value.push({
    text: transaction.text,
    amount: transaction.amount,
  });
  toast.success("Transaction added");
};

const handleTransactionDeleted = (index) => {
  transactions.value.splice(index, 1);
  toast.success("Transaction deleted");
};
</script>
