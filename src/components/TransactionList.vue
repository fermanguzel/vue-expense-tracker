<script setup lang="ts">
import { defineProps } from "vue";

const props = defineProps<{
  transactions: Array<{
    id: number;
    text: string;
    amount: number;
  }>;
}>();

const deleteTransaction = (id: number) => {};
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="{ minus: transaction.amount < 0, plus: transaction.amount >= 0 }"
    >
      {{ transaction.text }}
      <span>€{{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>
