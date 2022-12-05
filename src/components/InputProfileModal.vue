<script lang="ts" setup>
import Swal from "sweetalert2";
import { computed, reactive } from "vue";

import type Profile from "./Interfaces/Profile";

const props = defineProps<{
  value: Profile;
  title: string;
}>();

const emit = defineEmits(["input"]);

// reactiveで参照切りする
let profile: Profile = reactive(props.value);

const showProfiles = computed(() => {
  return `
  名前：${profile?.firstName ?? ""} ${profile?.lastName ?? ""}
  ニックネーム：${profile?.nickName ?? ""}
  血液型：${profile?.bloodType ?? ""}
  趣味：${profile?.hobby ?? ""}
  好きな食べ物：${profile?.favoriteFood ?? ""}
  `;
});

const createInputHTML = () => {
  return `
<div>
  <label for="first-name">名前</label>
  <div style=" display: grid; grid-template-columns: 50% 50%;">
    <input id="first-name" class="swal2-input" value="${
      profile?.firstName ?? ""
    }" style="margin-right: 2px; margin-left: 2px;">
    <input id="last-name" class="swal2-input" value="${
      profile?.lastName ?? ""
    }" style="margin-right: 2px; margin-left: 2px;">  
  </div>
</div>
<div>
  <label for="nick-name">あだ名</label>
  <div>
    <input id="nick-name" class="swal2-input" value="${
      profile?.nickName ?? ""
    }">
  </div>
</div>
<div>
  <label for="blood-type">血液型</label>
  <div>
    <input id="blood-type" class="swal2-input" value="${
      profile?.bloodType ?? ""
    }">
  </div>
</div>
<div>
  <label for="hobby">趣味</label>
  <div>
    <input id="hobby" class="swal2-input" value="${profile?.hobby ?? ""}">
  </div>
</div>
<div>
  <label for="favorite-food">好きな食べ物</label>
  <div>
    <input id="favorite-food" class="swal2-input" value="${
      profile?.favoriteFood ?? ""
    }">
  </div>
</div>
`;
};

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
    profile = <Profile>{
      firstName: result.value?.firstName ?? "",
      lastName: result.value?.lastName ?? "",
      nickName: result.value?.nickName ?? "",
      bloodType: result.value?.bloodType ?? "",
      hobby: result.value?.hobby ?? "",
      favoriteFood: result.value?.favoriteFood ?? "",
    };

    emit("input", profile);
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

<style scoped>
.full-name-wrapper {
  display: grid !important;
  grid-template-columns: 50% 50% !important;
}

#first-name {
  margin-right: 2px;
  margin-left: 2px;
}

#last-name {
  margin-right: 2px;
  margin-left: 2px;
}
</style>
