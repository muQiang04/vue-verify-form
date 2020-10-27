<template>
  <div>
    <div>
      <span
        style="display: inline-block;
              width: 200px;
              text-align: right;"
        >{{ label }}</span
      >
      <input v-bind="attrs" @blur="verification" v-model="val" />
    </div>
    <div v-if="err" style="color: red">{{ message }}</div>
  </div>
</template>

<script lang="ts">
import {
  computed,
  defineComponent,
  PropType,
  reactive,
  toRefs,
  onMounted
} from "vue";
import { emitter } from "./VerificationForm.vue";

export interface VerificationInputProps {
  type: "required" | "email";
  message: string;
}

const emailReg = /^([A-Za-z0-9_\-.])+@([A-Za-z0-9_\-.])+\.([A-Za-z]{2,4})$/;

export default defineComponent({
  name: "verificationInput",
  props: {
    rule: Array as PropType<VerificationInputProps[]>,
    modelValue: String
  },
  inheritAttrs: false,
  setup(props, context) {
    const { label, ...attrs } = context.attrs;

    const state = reactive({
      val: computed({
        get: () => props.modelValue || "",
        set: val => {
          context.emit("update:modelValue", val);
        }
      }),
      err: false,
      message: ""
    });

    const verification = () => {
      if (props.rule) {
        const allPassed = props.rule.every(rule => {
          const { type, message } = rule;
          let passed = true;
          state.message = message;

          switch (type) {
            case "required":
              passed = state.val.trim() !== "";
              break;
            case "email":
              passed = emailReg.test(state.val.trim());
              break;
            default:
              break;
          }
          return passed;
        });

        state.err = !allPassed;
        return allPassed;
      }

      return true;
    };

    onMounted(() => {
      emitter.emit("form-item-create", verification);
    });

    const data = toRefs(state);

    return {
      label,
      attrs,
      verification,
      ...data
    };
  }
});
</script>
