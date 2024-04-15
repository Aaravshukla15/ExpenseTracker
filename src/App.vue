<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <Transactionlist :transactions="transactions" 
      @transactionDeleted="handletransactionDeleted"
    />
    <AddTransaction
     @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpenses.vue';
  import Transactionlist from './components/Transactionlist.vue';
  import AddTransaction from './components/AddTransactions.vue';
  import { useToast } from 'vue-toastification';
import { ref, computed, onMounted } from 'vue';

  const toast = useToast();

const transactions = ref( []);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem
  ('transactions'));

  if(savedTransactions){
    transactions.value = savedTransactions;
  }
});

// Get Total
const total = computed(() =>{
    return transactions.value.reduce((acc,transaction) =>{
      return acc + transaction.amount;
    }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount >0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

// Get Expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
});

// add transaction
const handleTransactionSubmitted = (transactionData) =>{
transactions.value.push({
  id:genarateUniqueId(),
  text:transactionData.text,
  amount:transactionData.amount
});

  saveTransactionstoLocalStorage();
  toast.success('Transaction Added');
};

// generate id
const genarateUniqueId = ( ) =>{
  return Math.floor(Math.random() * 100000);
};

// transaction deleted
const handletransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => 
  transaction.id !==id);

  saveTransactionstoLocalStorage();

  toast.success('Transaction Deleted');
};

// save to loacl storage
const saveTransactionstoLocalStorage = () =>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>