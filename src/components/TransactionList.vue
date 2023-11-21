<template>
    <h3>Histórico</h3>
    <ul id="list" class="list">

        <!-- Renderização de itens de array em li é necessário passar o array através do v-for, onde opcao in opcoes será o nosso loop, e é necessário uma key para cada e podemos até adicionar classes condicionais -->

        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.text }} <span>${{ transaction.amount }}</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>
// export default {

// Função data retorna valores de lógicas que queremos aplicar no nosso template de forma dinâmica, no caso, estamos retornando um array de valores de objetos, mas este modo de aplicar data não é o mais atual.

// Modo data()
// data() {
//     return {
//         transactions: [
//             { id: 1, text: 'Flower', amount: -20 },
//             { id: 2, text: 'Salary', amount: 300 },
//             { id: 3, text: 'Book', amount: -10 },
//             { id: 4, text: 'Camera', amount: 150 }
//         ]
//     }
// }

// Modo setup()
// setup() {
// // Desta maneira determinados claramente as variáveis e seus valores de modo que fique mais fácil de trabalhar e reutiliza-las no template. Primeiro criamos os valores de array e depois retornamos um objeto com esses valores.

//     const transactions = [
//         { id: 1, text: 'Flower', amount: -20 },
//         { id: 2, text: 'Salary', amount: 300 },
//         { id: 3, text: 'Book', amount: -10 },
//         { id: 4, text: 'Camera', amount: 150 }
//     ]
//     return {
//         transactions
//     }
// }
// }

// Esta é a versão mais atual para renderizar dados no template, onde chamamos setup no script e não utilizaremos o export default. Aplicando direto as variáveis, funções e etc.

// const transactions = [
//     { id: 1, text: 'Flower', amount: -20 },
//     { id: 2, text: 'Salary', amount: 300 },
//     { id: 3, text: 'Book', amount: -10 },
//     { id: 4, text: 'Camera', amount: 150 }
// ]

// Para receber props precisamos definir dentro do script setup  uma função que receba propriedades. Para isso importaremos o defineProps e definiremos o nome do objeto que receberá as propriedades.

import { defineProps } from 'vue';

const emit = defineEmits(['transactionDeleted'])

const props = defineProps({
    transactions: {
        type: Array,
        required: true,

    }
})

// Criamos mais um evento para remover um objeto do array de transactions através do id.
const deleteTransaction = (id) => {
    emit('transactionDeleted', id)
}

</script>