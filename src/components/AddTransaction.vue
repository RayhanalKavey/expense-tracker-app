<script setup>
import { ref, defineEmits } from "vue";

const emit = defineEmits(["close", "toast"]);

const transactions = defineModel({
  type: Array,
  default: () => [],
});

const newTrack = ref({
  id: 0,
  title: "",
  amount: "",
  type: "",
});

function submitTransaction() {
  if (newTrack.value.amount <= 0) {
    alert("Amount must be greater than zero.");
    return;
  }

  newTrack.value.id = transactions.value.length + 1; //Create Id of new task
  newTrack.value.type = newTrack.value.type.toUpperCase(); // Convert type to uppercase
  transactions.value.unshift(newTrack.value); // Update list by pushing new transaction
  localStorage.setItem("transactions", JSON.stringify(transactions.value)); //Store in the local storage
  newTrack.value = {
    id: 0,
    title: "",
    amount: 0,
    type: "",
  }; //Reset input field

  emit("close");
  emit("toast", "Transaction added successfully!");
}
</script>

<template>
  <div class="bg-white rounded-lg p-6 w-80 w-full mx-auto">
    <h4 class="text-2xl font-semibold mb-4 text-center">Add Transaction</h4>
    <form @submit.prevent="submitTransaction" class="space-y-6">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2"
          >Title</label
        >
        <input
          v-model="newTrack.title"
          type="text"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          placeholder="Enter Title"
          required />
      </div>
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2"
          >Amount</label
        >
        <input
          v-model.number="newTrack.amount"
          type="number"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 no-arrows"
          placeholder="Enter Amount"
          required
          min="1" />
      </div>
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">Type</label>
        <select
          v-model="newTrack.type"
          class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          required>
          <option value="" disabled selected hidden class="text-gray-400">
            Select input type
          </option>
          <option value="Income">Income</option>
          <option value="Expense">Expense</option>
        </select>
      </div>
      <button
        type="submit"
        class="w-full bg-blue-500 text-white p-3 rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500">
        Add
      </button>
    </form>
  </div>
</template>

<style scoped>
.no-arrows::-webkit-inner-spin-button,
.no-arrows::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.no-arrows[type="number"] {
  -moz-appearance: textfield;
}

button:hover {
  background-color: #3182ce;
}

button:focus {
  box-shadow: 0 0 0 2px rgba(66, 153, 225, 0.5);
}
</style>
