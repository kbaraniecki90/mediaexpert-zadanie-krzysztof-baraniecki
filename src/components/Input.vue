<template>
  <div>
    <label :for="id" class="form-label">{{ label }}</label>
    <input
      :id="id"
      class="form-control"
      :type="type"
      :value="modelValue"
      :min="min"
      :placeholder="placeholder"
      @input="onInput"
    />
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  id: String,
  label: String,
  type: {
    type: String,
    default: "text",
  },
  modelValue: {
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

const emit = defineEmits(["update:modelValue"]);

const onInput = (event: Event) => {
  let value = (event.target as HTMLInputElement).value;

  if (props.type === "number") {
    value = value.replace(",", ".");
    const parsedValue = parseInt(value, 10);
    value = isNaN(parsedValue) ? value : Math.round(parsedValue);
  }

  emit("update:modelValue", value);
};
</script>
