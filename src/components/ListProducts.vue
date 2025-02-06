<script setup>
import { ref, computed, onMounted } from "vue";

const searchWord = ref('');
const searchCategory = ref('');
const categories = ref([]);

const props = defineProps({
  products: Array,
});

const filteredProducts = computed(() => {
  return props.products.filter(product => {
    return product.name.toLowerCase().includes(searchWord.value.toLowerCase())
      && (searchCategory.value == "" || product.category == searchCategory.value)
  });
});

defineEmits(['addToCart', 'deleteProduct']);

onMounted(async () => {
  const res = await fetch('http://localhost:3000/api/categories');
  categories.value = await res.json();
});
</script>

<template>
  <h1>Listado de Productos</h1>
  <p>
    Filtrar por 
    <input type="text" v-model="searchWord" />
  </p>
  <p>
    Seleccionar categoría: 
    <select v-model="searchCategory">
      <option value="">Cualquier categoría</option>
      <option v-for="(category,index) in categories" :key="index">
        {{ category }}
      </option>
    </select>
  </p>
  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Categoría</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in filteredProducts" 
          :key="product.id"
          :class="{warning: product.category == 'Alcoholic'}"
      >
        <td>{{ product.id }}</td>
        <td>{{ product.category }}</td>
        <td>{{ product.name }}</td>
        <td>{{ product.price }}</td>
        <td>
          <button @click="$emit('addToCart',product.id)">Añadir al carrito</button>
          <button @click="$emit('deleteProduct', product.id)">Elimninar Producto</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
h1 {
  color: blue;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 5px;
  text-align: center;}

th {
  background-color: lightblue;
}

tr:nth-child(even) {
  background-color: lightgray;
}

tr.warning {
  background-color: #ff6666;
}

tr.warning:nth-child(even) {
  background-color: #ff9999;
}


</style>