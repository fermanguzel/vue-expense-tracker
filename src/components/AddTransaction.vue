<script setup lang="ts">
import { ref } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");

const toast = useToast();

const emit = defineEmits(["transactionSubmitted"]);

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Both Fields Must Be Filled!");
  }

  const transactionData = {
    text: text.value,
    amount: amount.value,
  };

  emit("transactionSubmitted", transactionData);

  text.value = "";
  amount.value = "";
};
</script>
<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        v-model="text"
        placeholder="Enter text here..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />(negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount here..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
