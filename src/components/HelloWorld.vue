<script async setup lang="ts">
import { Product, Products } from "../types";
import axios from "axios";
import { onMounted, reactive, ref, watch } from "vue";

const products = reactive<Product[]>([]);
const searchKey = ref("");

onMounted(async () => {
  const res = await axios<Products>("https://dummyjson.com/products");
  products.push(...res.data.products);
});

watch(searchKey, (val, _, onCleanup) => {
  const timer = setTimeout(async () => {
    const res = await axios<Products>(
      `https://dummyjson.com/products/search?q=${val}`,
    );
    products.splice(0, products.length);
    products.push(...res.data.products);
  }, 500);

  onCleanup(() => {
    clearTimeout(timer);
  });
});
</script>

<template>
  <input v-model="searchKey" />
  <ul>
    <li v-for="product in products">{{ product.title }}</li>
  </ul>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
