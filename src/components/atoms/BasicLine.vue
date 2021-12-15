<template>
  <polygon
    :points="state.grabableArea.join(' ')"
    fill-opacity="0"
    fill="white"
    @mouseenter="state.isHover = true"
    @mouseleave="state.isHover = false"
    @mousedown="onMousedown"
    :style="{ cursor: state.isHover ? 'grab' : '' }"
  />
  <line
    :x1="state.x1"
    :y1="state.y1"
    :x2="state.x2"
    :y2="state.y2"
    :fill="color"
    :stroke="border"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";

export default defineComponent({
  name: "BasicLine",
  props: {
    x1: {
      type: Number,
      default: 0,
    },
    y1: {
      type: Number,
      default: 0,
    },
    x2: {
      type: Number,
      default: 0,
    },
    y2: {
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
    "update:x1",
    "update:y1",
    "update:x2",
    "update:y2",
    "grab",
    "mousedown",
  ],
  setup(props, { emit }) {
    const state = reactive({
      x1: computed({
        get: () => props.x1,
        set: (newValue) => emit("update:x1", newValue),
      }),
      y1: computed({
        get: () => props.y1,
        set: (newValue) => emit("update:y1", newValue),
      }),
      x2: computed({
        get: () => props.x2,
        set: (newValue) => emit("update:x2", newValue),
      }),
      y2: computed({
        get: () => props.y2,
        set: (newValue) => emit("update:y2", newValue),
      }),
      isHover: false,
      grabableArea: computed(() =>
        calcGrabableArea(props.x1, props.y1, props.x2, props.y2)
      ),
    });
    const distance = 10;
    const calcGrabableArea = (x1, y1, x2, y2) => {
      const rad = Math.atan2(y2 - y1, x2 - x1);
      const sinD = Math.sin(rad);
      const cosD = Math.cos(rad);
      return [
        [x1 - distance * (sinD + cosD), y1 - distance * (sinD - cosD)],
        [x1 + distance * (sinD - cosD), y1 - distance * (sinD + cosD)],
        [x2 + distance * (sinD + cosD), y2 + distance * (sinD - cosD)],
        [x2 - distance * (sinD - cosD), y2 + distance * (sinD + cosD)],
      ];
    };
    const onMousedown = (event) => {
      emit("mousedown", event);
    };
    return {
      state,
      onMousedown,
    };
  },
});
</script>
