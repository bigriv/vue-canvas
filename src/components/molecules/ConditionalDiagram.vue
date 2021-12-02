<template>
  <BasicPolygon
    v-model:points="state.points"
    :color="color"
    :border="border"
    @grab="onGrab"
  />
  <GrabArea
    v-if="state.isGrab"
    :x="state.grabableArea.x"
    :y="state.grabableArea.y"
    @release="onRelease"
    @mousemove="onMouseMove"
  />
</template>

<script>
import { defineComponent, reactive, computed, watch } from "vue";
import BasicPolygon from "@/components/atoms/BasicPolygon.vue";
import GrabArea from "@/components/atoms/GrabArea.vue";

export default defineComponent({
  name: "ConditionalDiagram",
  components: {
    BasicPolygon,
    GrabArea,
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
    "release",
  ],
  setup(props, { emit }) {
    const state = reactive({
      points: [
        [props.x, props.y + props.height / 2],
        [props.x + props.width / 2, props.y],
        [props.x + props.width, props.y + props.height / 2],
        [props.x + props.width / 2, props.y + props.height],
      ],
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
      isGrab: false,
      grabableArea: {
        x: 0,
        y: 0,
      },
    });
    const onMouseMove = (event) => {
      state.grabableArea.x += event.movementX;
      state.grabableArea.y += event.movementY;
      state.x += event.movementX;
      state.y += event.movementY;
    };
    const onGrab = (event) => {
      state.grabableArea.x = event.offsetX;
      state.grabableArea.y = event.offsetY;
      state.isGrab = true;
      emit("grab", event);
    };
    const onRelease = (event) => {
      state.isGrab = false;
      emit("release", event);
    };
    watch(
      () => [state.x, state.y],
      () =>
        (state.points = [
          [props.x, props.y + props.height / 2],
          [props.x + props.width / 2, props.y],
          [props.x + props.width, props.y + props.height / 2],
          [props.x + props.width / 2, props.y + props.height],
        ])
    );
    return {
      state,
      onMouseMove,
      onGrab,
      onRelease,
    };
  },
});
</script>
