<script setup lang="ts">
import { ref, reactive } from "vue";

import type Profile from "./components/Interfaces/Profile";

import InputModal from "./components/InputModal.vue";
import InputNameModal from "./components/InputNameModal.vue";
import InputProfileModal from "./components/InputProfileModal.vue";
import InputProfileModal2 from "./components/InputProfileModal2.vue";

const form = reactive({
  firstInput: "",
});

const name = ref({
  firstName: "",
  lastName: "",
});

let profile = reactive<Profile>({
  firstName: "",
  lastName: "",
  nickName: "",
  bloodType: "",
  hobby: "",
  favoriteFood: "",
});

const profile2 = ref<Profile>({
  firstName: "",
  lastName: "",
  nickName: "",
  bloodType: "",
  hobby: "",
  favoriteFood: "",
});

// reactiveでラップした中身でオブジェクト変数を管理すれば、オブジェクトごと代入した際にも変更を検知できる
const data = reactive({
  profile: <Profile>{
    firstName: "",
    lastName: "",
    nickName: "",
    bloodType: "",
    hobby: "",
    favoriteFood: "",
  },
});

const onInput = (e: Profile) => {
  // そのまま代入してしまうと、リアクティブ要素自体を更新してしまう。
  // オブジェクトごと変わってしまっているので、Vueで変更の検知ができなくなってしまう？
  // https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
  // profile = reactive(e);
  profile.firstName = e.firstName;
  profile.lastName = e.lastName;
  profile.nickName = e.nickName;
  profile.bloodType = e.bloodType;
  profile.hobby = e.hobby;
  profile.favoriteFood = e.favoriteFood;
};
</script>

<template>
  <main>
    <h1 class="green">Vue modal input</h1>

    <InputModal
      v-model="form.firstInput"
      title="文字列入力"
      label="ご自由に入力して下さい"
      style="margin-bottom: 5px"
    />

    <InputNameModal v-model="name" title="お名前" style="margin-bottom: 5px" />

    <InputProfileModal
      :value="profile"
      title="プロフィール"
      @input="onInput"
      style="margin-bottom: 5px"
    />

    <!-- ref変数を渡す場合は、暗黙的にrefのvalueを参照しているため、v-modeで渡してそのまま上書きされても、値の変更分を監視できる -->
    <InputProfileModal2
      v-model="profile2"
      title="プロフィール"
      style="margin-bottom: 5px"
    />

    <InputProfileModal2
      v-model="data.profile"
      title="プロフィール"
      style="margin-bottom: 5px"
    />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
