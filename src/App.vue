<script setup>
import Balance from './components/Balance.vue'
import InfoCard from './components/InfoCard.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])

const total = computed(() => transactions.value.reduce((acc, t) => acc + t.amount, 0))
const income = computed(() =>
  transactions.value.filter((t) => t.amount > 0).reduce((acc, t) => acc + t.amount, 0)
)
const expense = computed(() =>
  transactions.value.filter((t) => t.amount < 0).reduce((acc, t) => acc + t.amount, 0)
)

const handleAddTransaction = (transactionData) => {
  transactions.value.push({
    id: getUniqueID(),
    text: transactionData.text,
    amount: parseFloat(transactionData.amount)
  })
  saveTransactions()
  toast.success('Transaction added')
  console.log(transactions.value)
}

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id)
  saveTransactions()
  toast.success('Transaction deleted')
}

const saveTransactions = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

const getUniqueID = () => Math.floor(Math.random() * 10000)
</script>

<template>
  <h2 class="text-center">Expense Tracker</h2>
  <div class="container">
    <Balance :total="total" />
    <InfoCard :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" @deleteTransaction="handleDeleteTransaction" />
    <AddTransaction @addTransaction="handleAddTransaction" />
  </div>
</template>
