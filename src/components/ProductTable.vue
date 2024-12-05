<template>
  <div>
    <h2>Lista produktów</h2>
    <table class="table mt-3">
      <thead>
        <tr>
          <th>Nazwa produktu</th>
          <th>Ilość</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(product, index) in products" :key="index">
          <td>{{ product.name }}</td>
          <td>{{ product.quantity }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

export default defineComponent({
  props: {
    products: {
      type: Array as PropType<{ name: string; quantity: number }[]>,
      required: true,
    },
  },
});
</script>
