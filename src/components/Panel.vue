<template>
  <section class="container mt-5">
    <AddProduct @addProduct="handleAddProduct" />
  </section>

  <section class="container mt-4">
    <ProductTable :products="products" />
  </section>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import AddProduct from "./AddProduct.vue";
import ProductTable from "./ProductTable.vue";

export default defineComponent({
  components: {
    AddProduct,
    ProductTable,
  },
  data() {
    return {
      products: [
        { name: "Produkt 1", quantity: 1 },
        { name: "Produkt 2", quantity: 3 },
        { name: "Produkt 3", quantity: 2 },
      ],
    };
  },
  methods: {
    handleAddProduct(product: { name: string; quantity: number }) {
      this.products.push(product);
    },
  },
});
</script>
