<template>
    <h3>History</h3>
    <ul id='list' class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0? 'minus' : 'plus'">
         {{transaction.text}} <span>${{transaction.amount}}</span>
            <button class="delete-btn" @click="deleteTransaction(transaction.id)">X</button>
        </li>
        <!-- <li class="minus">
            Cash <span>-$400</span>
            <button class="delete-btn">X</button>
        </li>
        <li class="plus">
            Paycheck <span>$400</span>
            <button class="delete-btn">X</button>
        </li> -->
    </ul>
</template>

<!-- Older / Complex Way -->
<!-- <script>
export default {
    // Using Options API
    // data() {
    //     return {
    //         transactions: [
    //          {id: 1,text: 'Flower', amount: -19.99},   
    //          {id: 2,text: 'Salary', amount: 299.98},   
    //          {id: 3,text: 'Book', amount: -10},   
    //         ]
    //     }
    // }

    //Composition API
    setup() {
        const transactions = [
             {id: 1,text: 'Flower', amount: -19.99},   
             {id: 2,text: 'Salary', amount: 299.98},   
             {id: 3,text: 'Book', amount: -10},   
            ]

            return {transactions};
    }
}
</script> -->


<!-- Newer/Simplified Way -->
<!-- <script setup>
  const transactions = [
             {id: 1,text: 'Flower', amount: -19.99},   
             {id: 2,text: 'Salary', amount: 299.98},   
             {id: 3,text: 'Book', amount: -10},   
            ]
</script> -->

<script setup>

    const emit = defineEmits(['transactionDeleted']);
    const props = defineProps({
        transactions: {
        type: Array,
        required:true
        }
    });

    const deleteTransaction = (id) => {
        emit('transactionDeleted',id)
    }
</script>