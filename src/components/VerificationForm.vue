<template>
  <form>
    <slot></slot>
    <div
      style="display: inline-block;margin-top: 15px;"
      @click.prevent="submitForm"
    >
      <slot name="footer">
        <button>提交</button>
      </slot>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, onUnmounted } from "vue";
import mitt from "mitt";
type verifyFunc = () => boolean;

export const emitter = mitt();

export default defineComponent({
  emits: ["form-submit"],
  setup(props, context) {
    let funcArr: verifyFunc[] = [];

    const submitForm = () => {
      const result = funcArr.map(func => func()).every(res => res);
      context.emit("form-submit", result);
    };

    const callback = (func?: verifyFunc) => {
      if (func) {
        funcArr.push(func);
      }
    };

    emitter.on("form-item-create", callback);

    onUnmounted(() => {
      emitter.off("form-item-create", callback);
      funcArr = [];
    });

    return {
      submitForm
    };
  }
});
</script>
