<script lang="ts" setup>
import Swal from "sweetalert2";
import { computed } from "vue";

import type Profile from "./Interfaces/Profile";

const props = defineProps<{
  modelValue: Profile;
  title: string;
}>();

const emit = defineEmits(["update:modelValue"]);

const createInputHTML = () => {
  return `
<div>
  <label for="first-name">名前</label>
  <div style=" display: grid; grid-template-columns: 50% 50%;">
    <input id="first-name" class="swal2-input" value=":first-name" style="margin-right: 2px; margin-left: 2px;">
    <input id="last-name" class="swal2-input" value=":last-name" style="margin-right: 2px; margin-left: 2px;">  
  </div>
</div>
<div>
  <label for="nick-name">あだ名</label>
  <div>
    <input id="nick-name" class="swal2-input" value=":nick-name">
  </div>
</div>
<div>
  <label for="blood-type">血液型</label>
  <div>
    <input id="blood-type" class="swal2-input" value=":blood-type">
  </div>
</div>
<div>
  <label for="hobby">趣味</label>
  <div>
    <input id="hobby" class="swal2-input" value=":hobby">
  </div>
</div>
<div>
  <label for="favorite-food">好きな食べ物</label>
  <div>
    <input id="favorite-food" class="swal2-input" value=":favorite-food">
  </div>
</div>`
    .replace(":first-name", props.modelValue?.firstName ?? "")
    .replace(":last-name", props.modelValue?.lastName ?? "")
    .replace(":nick-name", props.modelValue?.nickName ?? "")
    .replace(":blood-type", props.modelValue?.bloodType ?? "")
    .replace(":hobby", props.modelValue?.hobby ?? "")
    .replace(":favorite-food", props.modelValue?.favoriteFood ?? "")
    .trim();
};

const showProfiles = computed(() => {
  return `
  名前：${props.modelValue?.firstName ?? ""} ${props.modelValue?.lastName ?? ""}
  ニックネーム：${props.modelValue?.nickName ?? ""}
  血液型：${props.modelValue?.bloodType ?? ""}
  趣味：${props.modelValue?.hobby ?? ""}
  好きな食べ物：${props.modelValue?.favoriteFood ?? ""}
  `;
});

const onClickField = async () => {
  const result = await Swal.fire({
    title: props.title,
    html: createInputHTML(),
    showCancelButton: true,
    preConfirm: () => {
      return {
        // @ts-ignore
        firstName: document.getElementById("first-name")?.value,
        // @ts-ignore
        lastName: document.getElementById("last-name")?.value,
        // @ts-ignore
        nickName: document.getElementById("nick-name")?.value,
        // @ts-ignore
        bloodType: document.getElementById("blood-type")?.value,
        // @ts-ignore
        hobby: document.getElementById("hobby")?.value,
        // @ts-ignore
        favoriteFood: document.getElementById("favorite-food")?.value,
      };
    },
  });

  if (result.isConfirmed) {
    const profile = <Profile>{
      firstName: result.value?.firstName ?? "",
      lastName: result.value?.lastName ?? "",
      nickName: result.value?.nickName ?? "",
      bloodType: result.value?.bloodType ?? "",
      hobby: result.value?.hobby ?? "",
      favoriteFood: result.value?.favoriteFood ?? "",
    };

    emit("update:modelValue", profile);
  }
};
</script>

<template>
  <textarea
    name="profile"
    id="profile"
    cols="30"
    rows="10"
    :value="showProfiles"
    readonly
    @click="onClickField"
    style="display: block"
  ></textarea>
</template>
