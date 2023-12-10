<template>
  <BasicLine
    v-model:x1="state.x1"
    v-model:y1="state.y1"
    v-model:x2="state.x2"
    v-model:y2="state.y2"
    :color="color"
    :border="border"
    @mousedown="onGrab"
  />
  <ConnectPoints v-model:points="points" @grab="onGrabPoint" />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";
import BasicLine from "@/components/atoms/BasicLine.vue";
import ConnectPoints from "@/components/molecules/ConnectPoints.vue";

export default defineComponent({
  name: "ConditionalDiagram",
  components: {
    BasicLine,
    ConnectPoints,
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
    length: {
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
    type: {
      type: String,
      default: "vertical_line",
      validator(val) {
        return ["vertical_line", "horizonal_line"].includes(val);
      },
    },
  },
  emits: ["update:x", "update:y", "update:length", "grab"],
  setup(props, { emit }) {
    const state = reactive({
      x1: computed({
        get: () => props.x,
        set: (newValue) => emit("update:x", newValue),
      }),
      y1: computed({
        get: () => props.y,
        set: (newValue) => emit("update:y", newValue),
      }),
      x2: computed(() =>
        props.type === "horizonal_line" ? props.x + props.length : props.x
      ),
      y2: computed(() =>
        props.type === "vertical_line" ? props.y + props.length : props.y
      ),
    });
    const points = computed(() => [
      { x: state.x1, y: state.y1 },
      { x: state.x2, y: state.y2 },
    ]);
    const onGrab = (event) => {
      emit("grab", event);
    };
    return {
      state,
      points,
      onGrab,
    };
  },
});
</script>
