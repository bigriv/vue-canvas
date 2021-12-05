<template>
  <polygon
    :points="state.points.join(' ')"
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
  name: "BasicPolygon",
  props: {
    points: {
      type: Array,
      default: () => [
        [0, 0],
        [0, 0],
        [0, 0],
      ],
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
  emits: ["update:points"],
  setup(props, { emit }) {
    const state = reactive({
      points: computed({
        get: () => props.points,
        set: (newValue) => emit("update:points", newValue),
      }),
      isHover: false,
    });
    return {
      state,
    };
  },
});
</script>
