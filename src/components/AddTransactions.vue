<template>
    <h3>
        Add New Transsaction
    </h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter Text..." />
        </div>
        <div class="form-control">
            <label for="text">Amount <br />(-ve Expense ,+ve Expense)</label>
            <input type="text" id="text" v-model="amount" placeholder="Enter Amount..." />
        </div>
        <button class="btn">Add Transaction</button>
    </form>
</template>

<script setup>
    import { ref } from 'vue';
    import {useToast} from 'vue-toastification';

    const text = ref('');
    const amount = ref('');
    const emit = defineEmits(['transactionsSubmitted']);
    const toast = useToast();


    const onSubmit = () =>{
        if(!text.value || !amount.value){
            toast.error("Both Feilds must be filled");
            return;
        }
        const transactionData = {
            text: text.value,
            amount:parseFloat(amount.value)
        }

        emit('transactionSubmitted',transactionData );

        text.value = "";
        amount.value="";
    };
</script>