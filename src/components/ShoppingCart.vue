<script setup>
import { computed } from "vue";

const props = defineProps({
  cartIds: Array,
});

defineEmits(['removeFromCart', 'incrementAmount', 'decrementAmount']);

const total = computed(() => {
  return props.cartIds.reduce((t,i) => t+=i.price*i.amount, 0);
})
</script>

<template>
  <h1>Carrito de la compra</h1>
  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Cantidad</th>
        <th>Subtotal</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cartId in cartIds" :key="cartId.id">
        <td>{{ cartId.id }}</td>
        <td>{{ cartId.name }}</td>
        <td>{{ parseFloat(cartId.price).toFixed(2) }}€</td>
        <td>
          {{ cartId.amount }}
          <button @click="$emit('decrementAmount', cartId.id)">-</button>
          <input type="number" v-model="cartId.amount">
          <button @click="$emit('incrementAmount', cartId.id)">+</button>
        </td>
        <td>{{ (cartId.price * cartId.amount).toFixed(2) }}€</td>
        <td>
          <button @click="$emit('removeFromCart', cartId.id)">Eliminar</button>
        </td>
      </tr>
    </tbody>
    <tfoot>
      <td colspan="4">Total</td>
      <td>{{ total.toFixed(2) }}€</td>
    </tfoot>
  </table>
</template>

<style scoped>

</style>