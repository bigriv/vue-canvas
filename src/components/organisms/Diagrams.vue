<template>
  <template v-for="figure in figures" :key="figure">
    <ConditionalDiagram
      v-if="figure.type === 'if'"
      v-model:x="figure.x"
      v-model:y="figure.y"
      v-model:width="figure.width"
      v-model:height="figure.height"
      :color="figure.color"
      :border="figure.border"
      @grab="onGrab($event, figure)"
    />
    <ProcessDiagram
      v-else-if="figure.type === 'process' || figure.type === 'function'"
      v-model:x="figure.x"
      v-model:y="figure.y"
      v-model:width="figure.width"
      v-model:height="figure.height"
      :color="figure.color"
      :border="figure.border"
      :type="figure.type"
      @grab="onGrab($event, figure)"
    />
    <StartAndEndDiagram
      v-else-if="figure.type === 'start' || figure.type === 'end'"
      v-model:x="figure.x"
      v-model:y="figure.y"
      v-model:width="figure.width"
      v-model:height="figure.height"
      :type="figure.type"
      @grab="onGrab($event, figure)"
    />
    <LoopDiagram
      v-else-if="figure.type === 'loop_start' || figure.type === 'loop_end'"
      v-model:x="figure.x"
      v-model:y="figure.y"
      v-model:width="figure.width"
      v-model:height="figure.height"
      :type="figure.type"
      @grab="onGrab($event, figure)"
    />
    <AssignmentDiagram
      v-else-if="figure.type === 'assign'"
      v-model:x="figure.x"
      v-model:y="figure.y"
      v-model:width="figure.width"
      v-model:height="figure.height"
      @grab="onGrab($event, figure)"
    />
    <ConnectLine
      v-else-if="
        figure.type === 'vertical_line' || figure.type === 'horizonal_line'
      "
      v-model:x="figure.x"
      v-model:y="figure.y"
      v-model:length="figure.length"
      :type="figure.type"
      @grab="onGrab($event, figure)"
    />
  </template>
  <GrabArea
    v-if="state.isGrab"
    :x="state.grabableArea.x"
    :y="state.grabableArea.y"
    @release="onRelease"
    @mousemove="onMouseMove($event)"
  />
</template>

<script>
import { defineComponent, reactive } from "vue";
import ConditionalDiagram from "@/components/molecules/ConditionalDiagram.vue";
import ProcessDiagram from "@/components/molecules/ProcessDiagram.vue";
import StartAndEndDiagram from "@/components/molecules/StartAndEndDiagram.vue";
import LoopDiagram from "@/components/molecules/LoopDiagram.vue";
import AssignmentDiagram from "@/components/molecules/AssignmentDiagram.vue";
import ConnectLine from "@/components/molecules/ConnectLine.vue";
import GrabArea from "@/components/atoms/GrabArea.vue";

export default defineComponent({
  name: "Diagrams",
  components: {
    ConditionalDiagram,
    ProcessDiagram,
    StartAndEndDiagram,
    LoopDiagram,
    AssignmentDiagram,
    ConnectLine,
    GrabArea,
  },
  props: {
    figures: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["grab", "release"],
  setup(props, { emit }) {
    const state = reactive({
      isGrab: false,
      grabFigure: undefined,
      grabableArea: {
        x: 0,
        y: 0,
      },
    });
    const onMouseMove = (event) => {
      state.grabableArea.x += event.movementX;
      state.grabableArea.y += event.movementY;
      state.grabFigure.x += event.movementX;
      state.grabFigure.y += event.movementY;
    };
    const onGrab = (event, figure) => {
      state.grabableArea.x = event.offsetX;
      state.grabableArea.y = event.offsetY;
      state.grabFigure = figure;
      state.isGrab = true;
      emit("grab", figure);
    };
    const onRelease = (figure) => {
      state.isGrab = false;
      emit("release", figure);
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
