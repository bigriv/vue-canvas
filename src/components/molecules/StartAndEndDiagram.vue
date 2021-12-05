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
    @mousedown="onGrab"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";
import BasicRect from "@/components/atoms/BasicRect.vue";

export default defineComponent({
  name: "ConditionalDiagram",
  components: {
    BasicRect,
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
