<template>
  <BasicRect
    v-model:x="state.x"
    v-model:y="state.y"
    v-model:rx="state.rx"
    v-model:ry="state.ry"
    v-model:width="state.width"
    v-model:height="state.height"
    :color="type === 'end' ? 'red' : 'lightgreen'"
    :border="'black'"
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
import { defineComponent, reactive, computed } from "vue";
import BasicRect from "@/components/atoms/BasicRect.vue";
import GrabArea from "@/components/atoms/GrabArea.vue";

export default defineComponent({
  name: "ConditionalDiagram",
  components: {
    BasicRect,
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
    type: {
      type: String,
      default: "start",
    }
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
      x: computed({
        get: () => props.x,
        set: (newValue) => emit("update:x", newValue),
      }),
      y: computed({
        get: () => props.y,
        set: (newValue) => emit("update:y", newValue),
      }),
      rx: computed(() => props.width / 4),
      ry: computed(() => props.height),
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
    return {
      state,
      onMouseMove,
      onGrab,
      onRelease,
    };
  },
});
</script>
