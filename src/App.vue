<script setup>
import { ref } from "vue";
import { onMounted } from "vue";
import ListProducts from "./components/ListProducts.vue";
import ShoppingCart from "./components/ShoppingCart.vue";

const username = ref('Andrés');
const products = ref([]);
const cartIds = ref([]);

onMounted(async () => {
  const res = await fetch('http://localhost:3000/api/products');
  products.value = await res.json();
});

function addToCart(productId) {
  const cartId = cartIds.value.find(element => element.id === productId);
  if (cartId) {
    cartId.amount++;
  } else {
    cartIds.value.push({ id: productId, amount: 1 });
  }
  console.log(cartIds.value);
}

function removeFromCart(id) {
  cartIds.value = cartIds.value.filter(cartId => cartId.id !== id);
}
</script>

<template>
  <h1>Práctica Framework Vue3</h1>
  <p><input type="text" v-model="username"></p>
  <p>Bienvenido {{ username }}</p>
  <p>Elementos del carrito: {{ cartIds.length }}</p>
  <ShoppingCart :cartIds="cartIds" 
    @removeFromCart="removeFromCart"
  />
  <ListProducts :products="products" @addToCart="addToCart" />
</template>

<style scoped>  
</style>
