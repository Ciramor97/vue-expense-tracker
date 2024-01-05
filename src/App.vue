<template>
  <Header />
  <div class="container">
    <Balance :balance="balance" />
    <IncomeExpense :incomes="incomes" :expenses="expenses" />
    <TransactionList
      :transactions="transactions"
      @transaction-deleted="handleTransactionDeleted"
    />
    <AddTransaction @submitted-data="handleTransactionDataSubmitted" />
  </div>
</template>
<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const transactions = ref([]);

const emit = defineEmits(["submittedData"]);

const toast = useToast();

const balance = computed(() => {
  return transactions.value
    .reduce((acc, prev) => acc + prev.amount, 0)
    .toFixed(2);
});

const incomes = computed(() => {
  return transactions.value
    .filter((transac) => transac.amount > 0)
    .reduce((acc, prev) => acc + prev.amount, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transac) => transac.amount < 0)
    .reduce((acc, prev) => acc + prev.amount, 0)
    .toFixed(2);
});

const handleTransactionDataSubmitted = (data) => {
  transactions.value.push({
    id: transactions.value.length + 1,
    ...data,
  });
  toast.success("Transaction added successfully");
  saveTransactionsToLocalStorage();
  console.log("transactions after pushing==", transactions.value);
};
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transac) => transac.id !== id
  );
  toast.success("Transaction deleted");
  saveTransactionsToLocalStorage();
};

function saveTransactionsToLocalStorage() {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
}
onMounted(() => {
  transactions.value = JSON.parse(localStorage.getItem("transactions")) ?? [];
});
</script>
