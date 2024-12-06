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
          <th>Akcje</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="sortedProducts.length === 0">
          <td colspan="3" class="text-center">Brak danych</td>
        </tr>

        <tr v-for="(product, index) in sortedProducts" :key="index">
          <td>{{ product.name }}</td>
          <td>{{ product.quantity }}</td>
          <td>
            <button
              @click="openModal(product, index)"
              class="btn btn-primary btn-sm"
            >
              Edytuj
            </button>
          </td>
        </tr>
      </tbody>
      <tfoot v-if="sortedProducts.length > 0">
        <tr>
          <td></td>
          <td>
            <strong>Razem: {{ totalQuantity }}</strong>
          </td>
          <td></td>
        </tr>
      </tfoot>
    </table>

    <ProductModal
      v-if="currentProduct"
      :show="isModalOpen"
      :product="currentProduct"
      @save="saveProduct"
      @close="closeModal"
      @delete="deleteProduct"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, computed, ref } from "vue";
import ProductModal from "./ProductModal.vue";

export default defineComponent({
  components: { ProductModal },
  props: {
    products: {
      type: Array as PropType<{ name: string; quantity: number }[]>,
      required: true,
    },
  },
  setup(props) {
    const sortKey = ref<string | null>(null);
    const sortOrder = ref<"asc" | "desc" | null>(null);
    const currentProduct = ref<{ name: string; quantity: number } | null>(null);
    const currentIndex = ref<number | null>(null);
    const isModalOpen = ref(false);

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

    const openModal = (
      product: { name: string; quantity: number },
      index: number
    ) => {
      currentProduct.value = { ...product };
      currentIndex.value = index;
      isModalOpen.value = true;
    };

    const closeModal = () => {
      isModalOpen.value = false;
      currentProduct.value = null;
    };

    const saveProduct = (updatedProduct: {
      name: string;
      quantity: number;
    }) => {
      if (currentIndex.value !== null) {
        props.products[currentIndex.value] = updatedProduct;
        closeModal();
      }
    };

    const deleteProduct = () => {
      if (currentIndex.value !== null) {
        props.products.splice(currentIndex.value, 1);
        closeModal();
      }
    };

    return {
      sortedProducts,
      sortBy,
      sortKey,
      sortOrder,
      totalQuantity,
      openModal,
      closeModal,
      saveProduct,
      deleteProduct,
      isModalOpen,
      currentProduct,
    };
  },
});
</script>
