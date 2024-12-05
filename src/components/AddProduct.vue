<template>
  <div>
    <h1>Dodawanie produktu</h1>
    <form @submit.prevent="addProduct">
      <div class="row align-items-end">
        <div class="col">
          <Input
            id="productName"
            label="Nazwa produktu"
            v-model="productName"
            placeholder="Nazwa produktu"
            required
          />
        </div>

        <div class="col">
          <Input
            id="productQuantity"
            type="number"
            label="Ilość produktu"
            placeholder="Ilość produktu"
            v-model="productQuantity"
            :min="1"
            required
          />
        </div>

        <div class="col">
          <button
            type="submit"
            class="btn btn-primary d-flex align-items-center gap-2"
            :disabled="isButtonDisabled"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              class="bi bi-plus"
              viewBox="0 0 16 16"
            >
              <path
                d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4"
              />
            </svg>
            Dodaj produkt
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Input from "./Input.vue";

export default defineComponent({
  components: {
    Input,
  },
  data() {
    return {
      productName: "",
      productQuantity: 1,
    };
  },
  computed: {
    isButtonDisabled(): boolean {
      return this.productName.length < 3;
    },
  },
  methods: {
    addProduct() {
      if (this.isButtonDisabled) return;

      const product = {
        name: this.productName,
        quantity: this.productQuantity,
      };

      this.$emit("addProduct", product);

      this.productName = "";
      this.productQuantity = 1;
    },
  },
});
</script>
