<script setup>
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, onMounted } from "vue";

const transactions = ref([]);

onMounted(() => {
  const storedTransactions =
    JSON.parse(localStorage.getItem("transactions")) || [];
  transactions.value = storedTransactions;
});
</script>

<template>
  <div class="container mx-auto p-6">
    <h2 class="text-center text-2xl font-bold">Expense Tracker</h2>
    <add-transaction v-model="transactions" />
    <transaction-list :transactions="transactions" />
  </div>
</template>

<style scoped></style>
