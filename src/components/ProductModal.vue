<template>
  <div v-if="show">
    <div
      :class="['modal fade', { show: show }]"
      :style="{ display: show ? 'block' : 'none' }"
      tabindex="-1"
      role="dialog"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header justify-content-between">
            <h5 class="modal-title">Edytuj produkt</h5>
            <button type="button" class="close" @click="closeModal">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="saveChanges">
              <div class="d-flex gap-3">
                <Input
                  id="name"
                  label="Nazwa produktu"
                  v-model="localProduct.name"
                  placeholder="Nazwa produktu"
                  required
                />
                <Input
                  id="quantity"
                  type="number"
                  label="Ilość produktu"
                  placeholder="Ilość produktu"
                  v-model="localProduct.quantity"
                  :min="1"
                  required
                />
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  @click="deleteProduct"
                  class="btn btn-danger"
                >
                  Usuń produkt
                </button>
                <button
                  type="button"
                  @click="closeModal"
                  class="btn btn-secondary"
                >
                  Anuluj
                </button>
                <button type="submit" class="btn btn-primary">
                  Aplikuj zmiany
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div :class="['modal-backdrop fade', { show: show }]"></div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import Input from "./Input.vue";

export default defineComponent({
  components: {
    Input,
  },
  props: {
    show: {
      type: Boolean,
      required: true,
    },
    product: {
      type: Object as () => { name: string; quantity: number },
      required: true,
    },
  },
  emits: ["save", "close", "delete"],
  setup(props, { emit }) {
    const localProduct = ref({ ...props.product });

    const closeModal = () => {
      emit("close");
    };

    const saveChanges = () => {
      emit("save", localProduct.value);
    };

    const deleteProduct = () => {
      emit("delete");
    };

    return {
      localProduct,
      closeModal,
      saveChanges,
      deleteProduct,
    };
  },
});
</script>
