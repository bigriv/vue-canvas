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
    type: {
      type: String,
      default: "loop_start",
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
      points: computed(() =>
        calcPoints(props.x, props.y, props.width, props.height)
      ),
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
    const calcPoints = (x, y, width, height) => {
      if (props.type === "loop_start") {
        return [
          [x, y + height / 3],
          [x + width / 4, y],
          [x + (width * 3) / 4, y],
          [x + width, y + height / 3],
          [x + width, y + height],
          [x, y + height],
        ];
      } else if (props.type === "loop_end") {
        return [
          [x, y],
          [x + width, y],
          [x + width, y + (height * 2) / 3],
          [x + (width * 3) / 4, y + height],
          [x + (width * 1) / 4, y + height],
          [x, y + (height * 2) / 3],
        ];
      }
    };
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
