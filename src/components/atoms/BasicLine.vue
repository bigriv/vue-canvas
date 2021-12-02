<template>
  <polygon
    :points="state.grabableArea.join(' ')"
    fill-opacity="0"
    fill="white"
    @mouseenter="state.isHover = true"
    @mouseleave="state.isHover = false"
    @mousedown="onGrab"
    @mouseup="onRelease"
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
import { defineComponent, reactive, computed, watch, onMounted } from "vue";

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
    "release",
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
      grabableArea: [
        [0, 0],
        [0, 0],
        [0, 0],
        [0, 0],
      ],
    });
    const distance = 10;
    const onGrab = (event) => {
      emit("grab", event);
    };
    const onRelease = (event) => {
      emit("release", event);
    };
    const calcGrabableArea = () => {
      const deg = Math.atan2(state.y2 - state.y1, state.x2 - state.x1)
      const sinD = Math.sin(deg);
      const cosD = Math.cos(deg);
      state.grabableArea = [
        [
          state.x1 - distance * (sinD + cosD),
          state.y1 + distance * (sinD - cosD),
        ],
        [
          state.x1 + distance * (sinD - cosD),
          state.y1 - distance * (sinD + cosD),
        ],
        [
          state.x2 + distance * (sinD + cosD),
          state.y2 - distance * (sinD - cosD),
        ],
        [
          state.x2 - distance * (sinD - cosD),
          state.y2 + distance * (sinD + cosD),
        ],
      ];
    }
    onMounted(() => {
      calcGrabableArea()
    });
    watch(
      () => [state.x1, state.y1, state.x2, state.y2],
      () => calcGrabableArea()
    );
    return {
      state,
      onGrab,
      onRelease,
    };
  },
});
</script>
