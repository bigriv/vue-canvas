<template>
  <circle
    :r="100"
    :cx="state.x"
    :cy="state.y"
    fill="white"
    fill-opacity="0"
    stroke="black"
    @mouseleave="onRelease"
    @mouseup="onRelease"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";

export default defineComponent({
  name: "BasicCircle",
  props: {
    x: {
      type: Number,
      default: 0,
    },
    y: {
      type: Number,
      default: 0,
    },
  },
  emits: ["update:r", "update:x", "update:y", "grab", "release"],
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
    });
    const onRelease = (event) => {
      emit("release", event);
    };
    return {
      state,
      onRelease,
    };
  },
});
</script>
