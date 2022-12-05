<script lang="ts" setup>
import Swal from "sweetalert2";

const props = defineProps<{
  modelValue: string;
  title: string;
  label: string;
}>();

const emit = defineEmits(["update:modelValue"]);

const onClickField = () => {
  Swal.fire({
    title: props.title,
    input: "text",
    inputLabel: props.label,
    inputValue: props.modelValue,
    showCancelButton: true,
  }).then((result) => {
    if (result.isConfirmed) {
      emit("update:modelValue", result.value);
    }
  });
};
</script>

<template>
  <input
    type="text"
    :value="props.modelValue"
    readonly
    @click="onClickField"
    style="display: block"
  />
</template>
