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
              <div class="form-group">
                <label for="name">Nazwa produktu</label>
                <input
                  v-model="localProduct.name"
                  id="name"
                  class="form-control"
                />
              </div>
              <div class="form-group">
                <label for="quantity">Ilość</label>
                <input
                  type="number"
                  v-model.number="localProduct.quantity"
                  id="quantity"
                  class="form-control"
                  min="1"
                />
              </div>
              <div class="modal-footer">
                <button type="submit" class="btn btn-primary">Zapisz</button>
                <button
                  type="button"
                  @click="closeModal"
                  class="btn btn-secondary"
                >
                  Anuluj
                </button>
                <button
                  type="button"
                  @click="deleteProduct"
                  class="btn btn-danger"
                >
                  Usuń produkt
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

export default defineComponent({
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
