<script setup>
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref } from "vue";

const transactions = ref([
  { id: 1, title: "Salary", type: "INCOME", amount: 5000 },
  { id: 2, title: "Freelance Work", type: "INCOME", amount: 1200 },
  { id: 3, title: "Investment Returns", type: "INCOME", amount: 800 },
  { id: 4, title: "Groceries", type: "EXPENSE", amount: 300 },
  { id: 5, title: "Rent", type: "EXPENSE", amount: 1500 },
  { id: 6, title: "Transportation", type: "EXPENSE", amount: 100 },
  { id: 7, title: "Dining Out", type: "EXPENSE", amount: 150 },
  { id: 8, title: "Utilities", type: "EXPENSE", amount: 200 },
  { id: 9, title: "Entertainment", type: "EXPENSE", amount: 120 },
  { id: 10, title: "Side Business", type: "INCOME", amount: 900 },
]);
</script>

<template>
  <div class="container mx-auto p-6">
    <h2 class="text-center text-2xl font-bold">Expense Tracker</h2>
    <add-transaction />
    <transaction-list :transactions="transactions" />
  </div>
</template>

<style scoped></style>
