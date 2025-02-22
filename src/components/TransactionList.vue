<script setup>
import { ref, computed, watch } from "vue";

const props = defineProps({
  transactions: Array, // ✅ Ensure transactions is received as a prop
});
const emit = defineEmits(["delete-transaction"]);

const getAmountClass = (amount, type) => {
  return {
    "text-green-500": type === "INCOME",
    "text-red-500": type === "EXPENSE",
    "font-bold": type === "EXPENSE" && amount >= 500,
  };
};
/* ===Pagination=== */
const currentPage = ref(props.transactions);
const itemsPerPage = 5;

// Compute total pages
const totalPages = computed(() =>
  Math.ceil(props.transactions.length / itemsPerPage)
);

// Extracting Transactions for the Current Page
const paginatedTransactions = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  return props.transactions.slice(start, start + itemsPerPage);
});
// Changing Pages
const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};
// console.log("transactions", transactions);
// console.log("paginatedTransactions", paginatedTransactions);
watch(
  () => props.transactions,
  () => {
    currentPage.value = 1;
  }
);
</script>

<template>
  <div class="mt-4">
    <h4 class="text-lg font-semibold">Transaction List</h4>
    <table class="w-full border-collapse border border-gray-300 mt-2">
      <thead>
        <tr class="bg-gray-100">
          <th class="border p-2">Title</th>
          <th class="border p-2">Amount</th>
          <th class="border p-2">Type</th>
          <th class="border p-2">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="props.transactions.length === 0">
          <td colspan="4" class="text-center p-2">
            No transactions recorded yet.
          </td>
        </tr>
        <tr
          v-for="transaction in paginatedTransactions"
          :key="transaction.id"
          class="border">
          <td class="border p-2">{{ transaction.title }}</td>
          <td
            :class="[
              'border p-2',
              getAmountClass(transaction.amount, transaction.type),
            ]">
            ${{ transaction.amount }}
          </td>
          <td class="border p-2">{{ transaction.type }}</td>
          <td class="border p-2">
            <button
              class="bg-red-500 text-white px-2 py-1 rounded"
              @click="emit('delete-transaction', transaction.id)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- Pagination Controls -->
    <div
      v-if="totalPages > 1"
      class="flex justify-center items-center gap-2 mt-4">
      <button
        class="px-3 py-1 bg-gray-300 rounded disabled:opacity-50"
        :disabled="currentPage === 1"
        @click="goToPage(currentPage - 1)">
        Prev
      </button>
      <button
        v-for="page in totalPages"
        :key="page"
        class="px-3 py-1 rounded"
        :class="{
          'bg-blue-500 text-white': page === currentPage,
          'bg-gray-300': page !== currentPage,
        }"
        @click="goToPage(page)">
        {{ page }}
      </button>
      <button
        class="px-3 py-1 bg-gray-300 rounded disabled:opacity-50"
        :disabled="currentPage === totalPages"
        @click="goToPage(currentPage + 1)">
        Next
      </button>
    </div>
  </div>
</template>

<style scoped></style>
