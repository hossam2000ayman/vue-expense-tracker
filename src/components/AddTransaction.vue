<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="text" type="text" id="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        v-model="amount"
        type="number"
        step="any"
        id="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const text = ref("");
const amount = ref(0);
const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value) {
    toast.error("text field are required");
    return;
  } else if (amount.value === 0) {
    toast.error("amount field must not be zero");
    return;
  }
  emit("transactionSubmitted", {
    text: text.value,
    amount: parseFloat(amount.value),
  });
  text.value = "";
  amount.value = 0;
};
</script>
