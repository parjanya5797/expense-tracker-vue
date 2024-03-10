<template>
<Header />
<div class="container">
<Balance :total="+total"/>
<IncomeExpense :income="+income" :expense="+expense" />
<TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
<AddTransaction @transactionSubmitted="handleTranasactionSubmitted"/>
</div>
</template>


<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from '@/components/AddTransaction.vue';

import {ref,computed, onMounted} from 'vue';

import {useToast} from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if(savedTransactions) {
        transactions.value = savedTransactions;
    }
}),

//Get Total
console.log(transactions.value);
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    },0).toFixed(2);
});

//GET INCOME 
const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc,transaction) =>{
        return acc + transaction.amount;
    },0).toFixed(2);
});

// GET EXPENSE
const expense = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc,transaction) => {
        return acc + transaction.amount;
    },0).toFixed(2) * -1;
})


//Add Transaction
const handleTranasactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
    });
    saveTransactionToLocalStorage();
    toast.success("Transaction Added Successfully.")
}

const generateUniqueId = () => {
    return Math.floor(Math.random() * 100000);
}

const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    saveTransactionToLocalStorage();
    toast.success("Transaction deleted successfully")
}

// Save to Local Storage
const saveTransactionToLocalStorage = () => {
    localStorage.setItem('transactions',JSON.stringify(transactions.value))
}
</script>