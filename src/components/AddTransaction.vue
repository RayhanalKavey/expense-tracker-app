<script setup>
import { ref } from "vue";

const transactions = defineModel({
  type: Array,
  default: () => [],
});

const newTrack = ref({
  id: 0,
  title: "",
  amount: 0,
  type: "",
});

function submitTransaction() {
  newTrack.value.id = transactions.value.length + 1; //Create Id of new task
  transactions.value.push(newTrack.value); // Update list by pushing new transaction

  localStorage.setItem("transactions", JSON.stringify(transactions.value)); //Store in the local storage

  newTrack.value = {
    id: 0,
    title: "",
    amount: 0,
    type: "",
  }; //Reset input field
}
</script>

<template>
  <div class="bg-white shadow-md rounded-lg p-4">
    <h4 class="text-xl font-semibold mb-2">Add Transaction</h4>
    <form
      @submit.prevent="submitTransaction"
      class="space-y-4 flex flex-col gap-4">
      <div>
        <label class="block">Title</label>
        <input
          v-model="newTrack.title"
          type="text"
          class="w-full p-2 border rounded-md"
          required />
      </div>
      <div>
        <label class="block">Amount</label>
        <input
          v-model.number="newTrack.amount"
          type="number"
          class="w-full p-2 border rounded-md"
          required
          min="1" />
      </div>
      <div>
        <label class="block">Type</label>
        <select
          v-model="newTrack.type"
          class="w-full p-2 border rounded-md"
          required>
          <option value="Income">Income</option>
          <option value="Expense">Expense</option>
        </select>
      </div>
      <button
        type="submit"
        class="w-full bg-blue-500 text-white p-2 rounded-md">
        Add
      </button>
    </form>
  </div>
</template>

<style scoped></style>
