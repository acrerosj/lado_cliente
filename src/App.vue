<script setup>
import { ref } from "vue";
import { onMounted } from "vue";
import { computed } from "vue";
import ListProducts from "./components/ListProducts.vue";
import ShoppingCart from "./components/ShoppingCart.vue";

const username = ref('Andrés');
const products = ref([]);
const cartIds = ref([]);
const orderField = ref('name');

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

function incrementAmount(id) {
  const cartId = cartIds.value.filter(cartId => cartId.id ===id )[0];
  cartId.amount++;
}

function decrementAmount(id) {
  const cartId = cartIds.value.filter(cartId => cartId.id ===id )[0];
  cartId.amount--;
  if (cartId.amount<=0) {
    removeFromCart(id);
  }

}

const cartProducts = computed(() => {
  return cartIds.value.map(item => {
    //const product = products.value.filter(p => p.id == item.id)[0];
    const product = products.value.find(p => p.id == item.id);
    item.name = product.name;
    item.price = product.price;
    return item;
  });  
});

const productsSort = computed(() => {
  if (orderField.value == 'name') {
    return products.value.sort((a,b) => a.name.localeCompare(b.name));
  }
  return products.value.sort((a,b) => a.price - b.price);
});

async function deleteProduct(id) {
  console.log("Eliminando: ", id);
  try {
    const res = await fetch('http://localhost:3000/api/products/'+id, {method: 'DELETE'});
    if (res.ok) {
      console.log('Eliminado');
      removeFromCart(id);
      products.value = products.value.filter(p => p.id != id);
    } else {
      throw('error al eliminar')
    }
  } catch (error) {
    console.log("ERROR DE LOS GORDOS: ", error);
  }
}
</script>

<template>
  <h1>Práctica Framework Vue3</h1>
  <p><input type="text" v-model="username"></p>
  <p>Bienvenido {{ username }}</p>
  <p>
    Elementos del carrito: {{ cartIds.length }}
  </p>
<p>
  Ordenar por: 
  <input type="radio" v-model="orderField" value="name" id="order-name">
  <label for="order-name">Nombre</label>
  <input type="radio" v-model="orderField" value="price" id="order-price">
  <label for="order-price">Precio</label>
  
</p>
  <ShoppingCart :cartIds="cartProducts" 
    @removeFromCart="removeFromCart"
    @incrementAmount="incrementAmount"
    @decrementAmount="decrementAmount"
  />
  <ListProducts :products="productsSort" 
      @addToCart="addToCart"
      @deleteProduct = "deleteProduct"
   />
</template>

<style scoped>  

</style>
