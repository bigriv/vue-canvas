<template>
  <BasicPolygon
    v-model:points="state.points"
    :color="color"
    :border="border"
    @mousedown="onGrab"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";
import BasicPolygon from "@/components/atoms/BasicPolygon.vue";

export default defineComponent({
  name: "ConditionalDiagram",
  components: {
    BasicPolygon,
  },
  props: {
    x: {
      type: Number,
      default: 0,
    },
    y: {
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
    "update:width",
    "update:height",
    "grab",
  ],
  setup(props, { emit }) {
    const state = reactive({
      points: computed(() => [
        [props.x, props.y + props.height / 2],
        [props.x + props.width / 2, props.y],
        [props.x + props.width, props.y + props.height / 2],
        [props.x + props.width / 2, props.y + props.height],
      ]),
      x: computed({
        get: () => props.x,
        set: (newValue) => emit("update:x", newValue),
      }),
      y: computed({
        get: () => props.y,
        set: (newValue) => emit("update:y", newValue),
      }),
      width: computed({
        get: () => props.width,
        set: (newValue) => emit("update:width", newValue),
      }),
      height: computed({
        get: () => props.height,
        set: (newValue) => emit("update:height", newValue),
      }),
    });
    const onGrab = (event) => {
      emit("grab", event);
    };
    return {
      state,
      onGrab,
    };
  },
});
</script>
