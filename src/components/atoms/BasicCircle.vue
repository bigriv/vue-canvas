<template>
  <circle
    :r="state.r"
    :cx="state.x"
    :cy="state.y"
    :fill="color"
    :stroke="border"
    :style="{ cursor: state.isHover ? 'grab' : '' }"
    @mouseenter="state.isHover = true"
    @mouseleave="state.isHover = false"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";

export default defineComponent({
  name: "BasicCircle",
  props: {
    r: {
      type: Number,
      default: 0,
    },
    x: {
      type: Number,
      default: 0,
    },
    y: {
      type: Number,
      default: 0,
    },
    isDraggable: {
      type: Boolean,
      default: false,
    },
    color: {
      type: String,
      default: "white",
    },
    border: {
      type: String,
      default: "black",
    },
  },
  emits: ["update:r", "update:x", "update:y"],
  setup(props, { emit }) {
    const state = reactive({
      r: computed({
        get: () => props.r,
        set: (newValue) => emit("update:r", newValue),
      }),
      x: computed({
        get: () => props.x,
        set: (newValue) => emit("update:x", newValue),
      }),
      y: computed({
        get: () => props.y,
        set: (newValue) => emit("update:y", newValue),
      }),
      isHover: false,
    });
    return {
      state,
    };
  },
});
</script>
