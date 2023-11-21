<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <!-- Versão atual para passar propriedades para um componente <Componente :propriedade="propriedade" /> -->
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDelete" />
    <AddTransaction @add-transaction="handleTransactionSubmitted" />
  </div>
</template>

<script setup>

// Não utilizaremos o export default. Faremos o modo atual de renderizar dados no template através do script setup. Desta maneira não há necessidade de retornar os componentes e sim chamá-los diretamente

import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';

import { ref, computed, onMounted } from 'vue';

const toast = useToast();

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
})

// No Vue.js, a propriedade computed é usada para criar propriedades computadas. Propriedades computadas são dependentes de uma ou mais propriedades reativas e são recalculadas automaticamente sempre que essas propriedades reativas são alteradas. Elas são úteis para realizar operações computacionais e transformações em dados de forma reativa.

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc += transaction.amount
  }, 0)
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc += transaction.amount
    }, 0).toFixed(2)
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc += transaction.amount
    }, 0).toFixed(2)
});
// Função para adicionar transação
const handleTransactionSubmitted = (transactionData) => {

  // A função handler segura os dados do transiction do form do componente AddTransaction, a partir disto iremos distruir esses dados através de um objeto que irá dar um push no array de transictions.

  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  });

  saveTransactionsToLocalStorage();

  toast.success('Valor adicionado!')
}
// Função para gerar ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000000);
}
// Função para remover transação da lista
const handleTransactionDelete = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

  saveTransactionsToLocalStorage();

  toast.success('Valor removido!')

}

// Salvar no localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

</script>