<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import { useToast } from "vue-toastification";
import AddTransaction from "./components/AddTransaction.vue";
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";

interface Transaction {
  id: number;
  text: string;
  amount: number;
}

const toast = useToast();
const transactions = ref<Transaction[]>([]);

onMounted(() => {
  const localTransactions = localStorage.getItem("transactions");

  if (localTransactions) {
    const savedTransactions = JSON.parse(localTransactions);
    transactions.value = savedTransactions;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

const handleTransactionSubmitted = (transactionData: Transaction) => {
  transactions.value.push({
    id: generateUniqeId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });

  saveTransactionsToLocalStorage();

  toast.success("Transaction Added!");
};

const handleTransactionDeleted = (id: number) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );

  saveTransactionsToLocalStorage();

  toast.warning("Transaction Deleted");
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

const generateUniqeId = () => {
  return Math.floor(Math.random() * 100000);
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
