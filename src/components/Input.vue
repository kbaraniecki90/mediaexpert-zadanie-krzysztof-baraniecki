<template>
  <div>
    <label :for="id" class="form-label">{{ label }}</label>
    <input
      :id="id"
      class="form-control"
      :type="type"
      :value="inputValue"
      :min="min"
      :placeholder="placeholder"
      @input="onInput"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps } from "vue";

const props = defineProps({
  id: String,
  label: String,
  type: {
    type: String,
    default: "text",
  },
  value: {
    type: [String, Number],
    default: "",
  },
  min: {
    type: [Number, String],
    default: 1,
  },
  required: {
    type: Boolean,
    default: false,
  },
  placeholder: {
    type: String,
    default: "",
  },
});

const inputValue = ref(props.value);

const onInput = (event: Event) => {
  let value = (event.target as HTMLInputElement).value;

  if (props.type === "number") {
    value = value = value.replace(",", ".");
    inputValue.value = value;
    const parsedValue = parseInt(value, 10);
    inputValue.value = isNaN(parsedValue) ? value : Math.round(parsedValue);
  }
};
</script>
