<script setup>
import { useToast } from 'vue-toastification'
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed } from 'vue'
let id = 0
const toast = useToast();
const transactions = ref([{ id: id++, text: "Flower", amount: -19.99 },
{ id: id++, text: "Salary", amount: 288.99 }, { id: id++, text: "Book", amount: -10.99 }]);
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2);
});


const handleTransaction = (transactionData) => {
  transactions.value.push({
    id: generateId(),
    text: transactionData.text,
    amount: transactionData.amount
  });
  toast.success('Transaction added');
}
const generateId = () => {

  const lastId = Math.max(...transactions.value.map(transaction => transaction.id));
  return lastId + 1;

}

const handleDeletion = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
  toast.success('Transaction Deleted');
  // console.log(id);
}
</script>




<template>
  {{ transactions }}
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleDeletion" />
    <AddTransaction @transactionSubmitted="handleTransaction" />
  </div>
</template>
