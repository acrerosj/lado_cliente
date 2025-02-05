<script setup>

const props = defineProps({
  cartIds: Array,
});

defineEmits(['removeFromCart']);

</script>

<template>
  <h1>Carrito de la compra</h1>
  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Cantidad</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="cartId in cartIds" :key="cartId.id">
        <td>{{ cartId.id }}</td>
        <td>{{ cartId.amount }}</td>
        <td>
          <button @click="$emit('removeFromCart', cartId.id)">Eliminar</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>

</style>