<template>
  <BasicRect
    v-model:x="state.outline.x"
    v-model:y="state.outline.y"
    v-model:width="state.outline.width"
    v-model:height="state.outline.height"
    :color="color"
    :border="border"
    @mousedown="onGrab"
  />
  <BasicRect
    v-if="type === 'function'"
    v-model:x="state.inline.x"
    v-model:y="state.inline.y"
    v-model:width="state.inline.width"
    v-model:height="state.inline.height"
    :color="color"
    :border="border"
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
      default: "process",
      validator(val) {
        return ["process", "function"].includes(val);
      },
    }
  },
  emits: ["update:x", "update:y", "update:width", "update:height", "grab"],
  setup(props, { emit }) {
    const state = reactive({
      outline: {
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
      },
      inline: {
        x: computed(() => props.x + props.width * 0.2),
        y: computed(() => props.y),
        width: computed(() => props.width * 0.6),
        height: computed(() => props.height),
      },
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
