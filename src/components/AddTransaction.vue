<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        placeholder="Enter text..."
        v-model="transactionData.text"
      />
    </div>
    <div class="form-control">
      <label for="amount">Amount <br /> </label>
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model.number="transactionData.amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { reactive } from "vue";
import { useToast } from "vue-toastification";

const emit = defineEmits(["submittedData"]);

const toast = useToast();

const transactionData = reactive({
  text: "",
  amount: "",
});

function onSubmit() {
  if (transactionData.text == "" || transactionData.amount == "") {
    toast.error("Both fields must be filled");
    return;
  } else {
    emit("submittedData", transactionData);
    console.log("transactionData==", transactionData.amount);
    // toast.success("")
    transactionData.text = "";
    transactionData.amount = "";
  }
}
</script>
