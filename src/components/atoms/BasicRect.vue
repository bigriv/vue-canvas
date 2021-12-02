<template>
  <rect
    :x="state.x"
    :y="state.y"
    :rx="state.rx"
    :ry="state.ry"
    :width="state.width"
    :height="state.height"
    :fill="color"
    :stroke="border"
    :style="{ cursor: state.isHover ? 'grab' : '' }"
    @mouseenter="state.isHover = true"
    @mouseleave="state.isHover = false"
    @mousedown="onGrab"
    @mouseup="onRelease"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";

export default defineComponent({
  name: "BasicRect",
  props: {
    x: {
      type: Number,
      default: 0,
    },
    y: {
      type: Number,
      default: 0,
    },
    rx: {
      type: Number,
      default: 0,
    },
    ry: {
      type: Number,
      default: 0,
    },
    width: {
      type: Number,
      default: 0,
    },
    height: {
      type: Number,
      default: 0,
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
  emits: [
    "update:x",
    "update:y",
    "update:rx",
    "update:ry",
    "update:width",
    "update:height",
    "grab",
    "release",
  ],
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
      rx: computed({
        get: () => props.rx,
        set: (newValue) => emit("update:rx", newValue),
      }),
      ry: computed({
        get: () => props.ry,
        set: (newValue) => emit("update:ry", newValue),
      }),
      width: computed({
        get: () => props.width,
        set: (newValue) => emit("update:width", newValue),
      }),
      height: computed({
        get: () => props.height,
        set: (newValue) => emit("update:height", newValue),
      }),
      isHover: false,
    });
    const onGrab = (event) => {
      emit("grab", event);
    };
    const onRelease = (event) => {
      emit("release"), event;
    };
    return {
      state,
      onGrab,
      onRelease,
    };
  },
});
</script>
