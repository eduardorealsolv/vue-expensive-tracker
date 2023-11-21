<template>
    <h3>Adicionar nova transação</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Texto</label>
            <input type="text" id="text" placeholder="Insira o texto" v-model="text" />
        </div>
        <div class="form-control">
            <label for="amount">
                Valor <br />(Para despesas utilize -, para receitas utilize +)</label>
            <input type="text" id="amount" placeholder="Insira o valor" v-model="amount" />
        </div>
        <button class="btn">Adicionar</button>
    </form>
</template>

<script setup>

import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('')
const amount = ref('')
// Para definir eventos em um componente, utilizamos o defineEmits, e passamos um array com os nomes dos eventos que serão emitidos.

const emit = defineEmits(['add-transaction'])

const toast = useToast();
const onSubmit = () => {
    if (!text.value || !amount.value) {
        toast.error('Por favor, preencha todos os campos');
        return
    }
    const transactionData = {
        text: text.value,
        amount: parseFloat(amount.value)
    }

    // O primeiro argumento de emit é o nome do evento e o segundo argumento o valor que será passado.

    emit('add-transaction', transactionData)

    // Zerar valores dos inputs para que o form seja limpo.

    text.value = ''
    amount.value = ''
}
</script>