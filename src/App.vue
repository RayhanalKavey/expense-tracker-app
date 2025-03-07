<script setup>
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, onMounted, computed, watch } from "vue";

const transactions = ref([]);

// Add transaction modal state
const add = ref(false);

// Filter state
const selectedFilter = ref("ALL");

// Delete confirmation modal state
const deleteModal = ref(false);
const transactionToDelete = ref(null);

// Toast Notification
const toastMessage = ref("");
const showToast = ref(false);

onMounted(() => {
  const storedTransactions =
    JSON.parse(localStorage.getItem("transactions")) || [];
  transactions.value = storedTransactions;
});

// Computed property to filter transactions based on the selected filter
const filteredTransactions = computed(() => {
  if (selectedFilter.value === "INCOME") {
    return transactions.value.filter((t) => t.type === "INCOME");
  } else if (selectedFilter.value === "EXPENSE") {
    return transactions.value.filter((t) => t.type === "EXPENSE");
  }
  return transactions.value;
});
// Open delete confirmation modal
const confirmDelete = (id) => {
  transactionToDelete.value = id;
  deleteModal.value = true;
};
// Delete a transaction based of id after confirmation
const deleteTransaction = () => {
  if (transactionToDelete.value !== null) {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== transactionToDelete.value
    );
    localStorage.setItem("transactions", JSON.stringify(transactions.value));
    // Show toast message for successful deletion
    showToastMessage("Transaction deleted successfully!");
  }
  deleteModal.value = false;
  transactionToDelete.value = null;
};
// Show toast message
const showToastMessage = (message) => {
  toastMessage.value = message;
  showToast.value = true;
  setTimeout(() => {
    showToast.value = false;
  }, 2000); // Hide toast after 2 seconds
};
const currentPage = ref(1);
watch(selectedFilter, () => {
  currentPage.value = 1;
});
</script>

<template>
  <div class="container mx-auto p-6">
    <h1 class="text-center text-6xl font-bold mb-6">Expense Tracker</h1>

    <!--Add Modal -->
    <div
      v-if="add"
      class="fixed inset-0 bg-black/30 flex justify-center items-center p-4">
      <div
        class="bg-white p-6 rounded-lg w-full max-w-md sm:max-w-lg lg:max-w-xl relative">
        <button
          @click="add = false"
          class="absolute top-3 right-5 text-gray-500 hover:text-red-500 text-5xl">
          &times;
        </button>
        <add-transaction
          v-model="transactions"
          @close="add = false"
          @toast="showToastMessage" />
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
    <!-- Transaction list -->
    <transaction-list
      :transactions="filteredTransactions"
      :current-page="currentPage"
      @delete-transaction="confirmDelete" />

    <!-- Delete Confirmation Modal -->
    <div
      v-if="deleteModal"
      class="fixed inset-0 bg-black/30 flex justify-center items-center p-4">
      <div class="bg-white p-6 rounded-lg w-full max-w-md text-center">
        <h2 class="text-2xl font-bold mb-4">Confirm Delete</h2>
        <p class="mb-4">Are you sure you want to delete this transaction?</p>
        <div class="flex justify-center gap-4">
          <button
            @click="deleteTransaction"
            class="bg-red-500 text-white px-4 py-2 rounded-md">
            Yes, Delete
          </button>
          <button
            @click="deleteModal = false"
            class="bg-gray-300 px-4 py-2 rounded-md">
            Cancel
          </button>
        </div>
      </div>
    </div>

    <!-- Toast Notification -->
    <div
      v-if="showToast"
      class="fixed bottom-5 right-5 bg-green-500 text-white px-4 py-3 rounded-md shadow-lg transition-opacity duration-500">
      {{ toastMessage }}
    </div>
  </div>
</template>

<style scoped></style>
