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
      <tfoot>
        <tr>
          <td><strong>Razem:</strong></td>
          <td>
            <strong>{{ totalQuantity }}</strong>
          </td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, computed } from "vue";

export default defineComponent({
  props: {
    products: {
      type: Array as PropType<{ name: string; quantity: number }[]>,
      required: true,
    },
  },
  setup(props) {
    const totalQuantity = computed(() =>
      props.products.reduce((total, product) => total + product.quantity, 0)
    );

    return {
      totalQuantity,
    };
  },
});
</script>
