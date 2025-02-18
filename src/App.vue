<script setup>
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, onMounted, computed } from "vue";

const transactions = ref([]);
const add = ref(false);
const selectedFilter = ref("ALL");

onMounted(() => {
  const storedTransactions =
    JSON.parse(localStorage.getItem("transactions")) || [];
  transactions.value = storedTransactions;
});

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

// Computed property to filter transactions based on the selected filter
const filteredTransactions = computed(() => {
  if (selectedFilter.value === "INCOME") {
    return transactions.value.filter((t) => t.type === "Income");
  } else if (selectedFilter.value === "EXPENSE") {
    return transactions.value.filter((t) => t.type === "Expense");
  }
  return transactions.value;
});
</script>

<template>
  <div class="container mx-auto p-6">
    <h1 class="text-center text-6xl font-bold mb-6">Expense Tracker</h1>

    <!-- Modal -->
    <div
      v-if="add"
      class="fixed inset-0 bg-black/30 flex justify-center items-center p-4">
      <!-- Modal Container (No Shadow) -->
      <div
        class="bg-white p-6 rounded-lg w-full max-w-md sm:max-w-lg lg:max-w-xl relative">
        <!-- Close Button -->
        <button
          @click="add = false"
          class="absolute top-3 right-5 text-gray-500 hover:text-red-500 text-5xl">
          &times;
        </button>
        <add-transaction v-model="transactions" @close="add = false" />
      </div>
    </div>
    <!-- Filter Buttons -->

    <div class="flex justify-between mt-4">
      <div class="flex gap-2">
        <button
          @click="selectedFilter = 'ALL'"
          :class="[
            'px-4 py-2 rounded',
            selectedFilter === 'ALL' ? 'bg-blue-500 text-white' : 'bg-gray-200',
          ]">
          All Transactions
        </button>
        <button
          @click="selectedFilter = 'INCOME'"
          :class="[
            'px-4 py-2 rounded',
            selectedFilter === 'INCOME'
              ? 'bg-green-500 text-white'
              : 'bg-gray-200',
          ]">
          Incomes
        </button>
        <button
          @click="selectedFilter = 'EXPENSE'"
          :class="[
            'px-4 py-2 rounded',
            selectedFilter === 'EXPENSE'
              ? 'bg-red-500 text-white'
              : 'bg-gray-200',
          ]">
          Expenses
        </button>
      </div>
      <button
        @click="add = true"
        class="bg-blue-500 text-white px-4 py-2 rounded-md">
        New Transaction
      </button>
    </div>
    <transaction-list
      :transactions="filteredTransactions"
      @delete-transaction="deleteTransaction" />
  </div>
</template>

<style scoped></style>
