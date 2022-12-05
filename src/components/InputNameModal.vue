<script lang="ts" setup>
import Swal from "sweetalert2";
import { computed } from "vue";

const props = defineProps<{
  modelValue: {
    firstName: string;
    lastName: string;
  };
  title: string;
}>();

const emit = defineEmits(["update:modelValue"]);

const html = `
<div>
  <label for="first-name">苗字</label>
  <input id="first-name" class="swal2-input" value="${props.modelValue.firstName}">
</div>
<div>
  <label for="last-name">名前</label>
  <input id="last-name" class="swal2-input" value="${props.modelValue.lastName}">
</div>
`;

const showFullName = computed(() => {
  const first = props.modelValue.firstName.trim();
  const last = props.modelValue.lastName.trim();

  if (first.length < 1) {
    return last;
  }

  return `${first} ${last}`;
});

const onClickField = async () => {
  const result = await Swal.fire({
    title: props.title,
    html: html,
    showCancelButton: true,
    preConfirm: () => {
      return [
        // @ts-ignore
        document.getElementById("first-name")?.value,
        // @ts-ignore
        document.getElementById("last-name")?.value,
      ];
    },
  });

  if (result.isConfirmed) {
    if (result.value && result.value?.length === 2) {
      emit("update:modelValue", {
        firstName: result?.value[0],
        lastName: result?.value[1],
      });
    }
  }
};
</script>

<template>
  <input
    type="text"
    :value="showFullName"
    readonly
    @click="onClickField"
    style="display: block"
  />
</template>
