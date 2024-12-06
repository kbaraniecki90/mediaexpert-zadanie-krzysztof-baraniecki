<template>
  <div>
    <h2>Lista produktów</h2>
    <table class="table table-striped mt-3">
      <thead>
        <tr>
          <th @click="sortBy('name')">
            Nazwa produktu
            <span v-if="sortKey === 'name' && sortOrder === 'asc'">↑</span>
            <span v-if="sortKey === 'name' && sortOrder === 'desc'">↓</span>
          </th>
          <th @click="sortBy('quantity')">
            Ilość
            <span v-if="sortKey === 'quantity' && sortOrder === 'asc'">↑</span>
            <span v-if="sortKey === 'quantity' && sortOrder === 'desc'">↓</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(product, index) in sortedProducts" :key="index">
          <td>{{ product.name }}</td>
          <td>{{ product.quantity }}</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td></td>
          <td>
            <strong>Razem: {{ totalQuantity }}</strong>
          </td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, computed, ref } from "vue";

export default defineComponent({
  props: {
    products: {
      type: Array as PropType<{ name: string; quantity: number }[]>,
      required: true,
    },
  },
  setup(props) {
    const sortKey = ref<string | null>(null);
    const sortOrder = ref<"asc" | "desc" | null>(null);

    const sortedProducts = computed(() => {
      if (!sortKey.value) return props.products;

      const sorted = [...props.products].sort((a, b) => {
        if (sortKey.value === "name") {
          return a.name.localeCompare(b.name);
        } else if (sortKey.value === "quantity") {
          return a.quantity - b.quantity;
        }
        return 0;
      });

      if (sortOrder.value === "desc") {
        return sorted.reverse();
      }

      return sorted;
    });

    const sortBy = (key: string) => {
      if (sortKey.value === key) {
        if (sortOrder.value === "asc") {
          sortOrder.value = "desc";
        } else if (sortOrder.value === "desc") {
          sortKey.value = null;
          sortOrder.value = null;
        } else {
          sortOrder.value = "asc";
        }
      } else {
        sortKey.value = key;
        sortOrder.value = "asc";
      }
    };

    const totalQuantity = computed(() =>
      props.products.reduce((total, product) => total + product.quantity, 0)
    );

    return {
      sortedProducts,
      sortBy,
      sortKey,
      sortOrder,
      totalQuantity,
    };
  },
});
</script>
