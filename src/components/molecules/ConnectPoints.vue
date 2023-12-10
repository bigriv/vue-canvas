<template>
  <!-- TODO: カーソルタイプを変える -->
  <BasicCircle
    v-for="(point, index) in state.points"
    v-model:x="point.x"
    v-model:y="point.y"
    :r="5"
    :color="'white'"
    :border="'black'"
    :key="point"
    :style="{ cursor: state.isHover ? 'text' : '' }"
    @mousedown="onGrab($event, index)"
    @mouseenter="state.isHover = true"
    @mouseleave="state.isHover = false"
  />
</template>

<script>
import { defineComponent, reactive, computed } from "vue";
import BasicCircle from "@/components/atoms/BasicCircle.vue";

export default defineComponent({
  name: "AssignmentDiagram",
  components: {
    BasicCircle,
  },
  props: {
    points: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["update:points", "grab"],
  setup(props, { emit }) {
    const state = reactive({
      isHover: false,
      points: computed({
        get: () => props.points,
        set: (newValue) => emit("update:points", newValue),
      }),
    });
    const onGrab = (event, index) => {
      emit("grab", event, index);
    };
    return {
      state,
      onGrab,
    };
  },
});
</script>
