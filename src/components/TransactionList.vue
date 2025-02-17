<script setup>
defineProps({
  transactions: Array,
});

const getAmountClass = (amount, type) => {
  return {
    "text-green-500": type === "INCOME",
    "text-red-500": type === "EXPENSE",
    "font-bold": type === "EXPENSE" && amount >= 500,
  };
};
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
        <tr v-if="transactions.length === 0">
          <td colspan="4" class="text-center p-2">
            No transactions recorded yet.
          </td>
        </tr>
        <tr
          v-for="(transaction, index) in transactions"
          :key="index"
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
              @click="$emit('delete-transaction', index)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped></style>
