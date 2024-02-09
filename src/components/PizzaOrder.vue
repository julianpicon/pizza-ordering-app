<template>
    <div class="container">

        <div class="section" id="orderSection">
            <h1>Welcome to Our Pizza Store</h1>
            <div class="pizza-options">
                <div v-for="pizza in pizzas" :key="pizza.name" class="pizza-option"
                    :class="{ 'selected': selectedPizza === pizza.name }" @click="selectPizza(pizza.name)">
                    <img :src="pizza.image" :alt="pizza.name">
                    <p>{{ pizza.name }}</p>
                </div>
            </div>
            <button id="orderButton" @click="placeOrder" :disabled="!selectedPizza">Place Order</button>
        </div>

        <div class="section" id="statusSection">
            <h2>Order Status</h2>
            <div id="statusUpdates">Please select a pizza to place your order...</div>
        </div>
    </div>
</template>
    
<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { ENDPOINTS } from '@/apiConfig';

const pizzas = ref([
    { name: "Margherita", image: require("../assets/margherita.jpg") },
    { name: "Pepperoni", image: require("../assets/pepperoni.jpg") },
    { name: "Vegetarian", image: require("../assets/vegetarian.jpg") },
    { name: "Hawaiian", image: require("../assets/hawaiian.jpg") },
    { name: "Meat Lover's", image: require("../assets/meatlovers.jpg") }
]);
const selectedPizza = ref(null);
const statusUpdates = ref("Please select a pizza to place your order...");

const selectPizza = (pizza) => {
    selectedPizza.value = pizza;
    console.log(pizza + '2');
};

const placeOrder = () => {
    // Call the backend endpoint to place the order
    axios.post(ENDPOINTS.placeOrder, { clientId: 1, pizzaType: selectedPizza.value })
        .then(response => {
            console.log('Order placed successfully', response.data);
            subscribeToOrderUpdates(response.data.orderId);
        })
        .catch(error => {
            console.error('Error placing order:', error);
            statusUpdates.value += "<br>Failed to place order. Please try again.";
        });
};

const subscribeToOrderUpdates = (orderId) => {
    alert(orderId);
    // WebSocket logic goes here
};

</script>

<style scoped>
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
}

.container {
    width: 80%;
    margin: 20px auto;
    text-align: center;
}

.section {
    padding: 20px;
    background-color: white;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
}

.pizza-options {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}

.pizza-option {
    margin: 10px;
    border: 2px solid transparent;
    border-radius: 10px;
    cursor: pointer;
}

.pizza-option:hover,
.pizza-option.selected {
    border-color: #007bff;
}

.pizza-option img {
    width: 150px;
    height: 150px;
    border-radius: 10px;
}

button {
    padding: 10px 20px;
    margin-top: 20px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

#statusUpdates {
    margin-top: 20px;
    padding: 10px;
    background-color: #eee;
    border-radius: 10px;
}
</style>

    