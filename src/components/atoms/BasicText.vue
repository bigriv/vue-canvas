<template>
  <text
    v-if="!state.isFocus"
    :x="state.x"
    :y="state.y"
    @click="state.isFocus = true"
  >
    {{ state.text }}
  </text>
  <foreignObject>
    <teleport to="#input-area">
      <input
        v-if="state.isFocus"
        :style="state.position"
        class="text-area"
        type="text"
        v-model="state.text"
        @blur="state.isFocus = false"
        v-focus
      />
    </teleport>
  </foreignObject>
</template>

<script>
import { defineComponent, reactive, computed } from "vue";

export default defineComponent({
  name: "BasicText",
  props: {
    x: {
      type: Number,
      default: 0,
    },
    y: {
      type: Number,
      default: 0,
    },
    text: {
      type: String,
      default: "",
    },
  },
  emits: ["update:x", "update:y", "update:text"],
  setup(props, { emit }) {
    const state = reactive({
      x: computed({
        get: () => props.x,
        set: (newValue) => emit("update:x", newValue),
      }),
      y: computed({
        get: () => props.y,
        set: (newValue) => emit("update:y", newValue),
      }),
      text: computed({
        get: () => props.text,
        set: (newValue) => emit("update:text", newValue),
      }),
      position: computed(() => {
        return {
          left: props.x + "px",
          top: props.y - 20 + "px",
        };
      }),
      isFocus: false,
    });
    return {
      state,
    };
  },
  directives: {
    focus: {
      mounted: function (el) {
        el.focus();
      },
    },
  },
});
</script>
<style scoped>
.text-area {
  position: absolute;
  border: 1px black solid;
}
</style>
