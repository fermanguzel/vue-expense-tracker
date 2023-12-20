<script setup lang="ts">
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

interface Transaction {
  id: number;
  text: string;
  amount: number;
}

const transactions = ref<Transaction[]>([
  { id: 1, text: "Macbook Pro", amount: -499.99 },
  { id: 2, text: "MSI Notebook", amount: -299.99 },
  { id: 3, text: "Salary", amount: 1600.0 },
  { id: 4, text: "Iphone 16 Pro", amount: -399.99 },
  { id: 5, text: "Tip", amount: 600.0 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc: number, transaction: Transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc: number, transaction: Transaction) => {
      return acc + transaction.amount;
    }, 0);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc: number, transaction: Transaction) => {
      return acc + transaction.amount;
    }, 0);
});

const handleTransactionSubmitted = (transactionData: Transaction) => {
  transactions.value.push({
    id: generateUniqeId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  toast.success("Transaction Added!");
};

const generateUniqeId = () => {
  return Math.floor(Math.random() * 100000);
};
</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
