<template>
  <verification-form @form-submit="onFormSubmit">
    <verification-input
      label="email："
      placeholder="请输入email"
      :rule="emailRule"
      v-model="eamilInit"
    />
    <verification-input
      label="userName："
      placeholder="请输入userName"
      :rule="userRule"
      v-model="userInit"
      class="input"
    />
    <template #footer>
      <button>test</button>
    </template>
  </verification-form>

  <span>{{ testRef }}</span>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import VerificationInput from "./components/VerificationInput.vue";
import VerificationForm from "./components/VerificationForm.vue";

export default defineComponent({
  name: "App",
  components: {
    VerificationInput,
    VerificationForm
  },
  setup() {
    const emailRule = ref([
      { type: "required", message: "请输入邮箱" },
      { type: "email", message: "邮箱格式错误" }
    ]);

    const eamilInit = ref("");

    const userRule = ref([{ type: "required", message: "请输入用户名" }]);

    const userInit = ref("");

    const onFormSubmit = () => {
      return true;
    };

    return {
      emailRule,
      eamilInit,
      userRule,
      userInit,
      onFormSubmit
    };
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.input {
  margin-top: 15px;
}
</style>
