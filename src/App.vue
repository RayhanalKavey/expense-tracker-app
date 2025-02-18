<script setup>
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, onMounted } from "vue";

const transactions = ref([]);
const add = ref(false);

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
</script>

<template>
  <div class="container mx-auto p-6">
    <h1 class="text-center text-6xl font-bold mb-6">Expense Tracker</h1>
    <button
      @click="add = true"
      class="bg-blue-500 text-white px-4 py-2 rounded-md">
      New Transaction
    </button>

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

    <transaction-list
      :transactions="transactions"
      @delete-transaction="deleteTransaction" />
  </div>
</template>

<style scoped></style>
