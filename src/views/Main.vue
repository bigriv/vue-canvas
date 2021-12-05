<template>
  <div class="mb-10">
  <FigureToolBar @click="addDiagram" />
  <Canvas :figures="state.figures" />
  <!-- <GridCanvas /> -->
  </div>
</template>
<script>
import { defineComponent, reactive } from "vue";
import FigureToolBar from "@/components/template/FigureToolBar.vue";
import Canvas from "@/components/template/Canvas.vue";
// import GridCanvas from "@/components/template/GridCanvas.vue";

export default defineComponent({
  name: "Main",
  components: {
    FigureToolBar,
    Canvas,
    // GridCanvas
  },
  setup() {
    const state = reactive({
      figures: [],
    });
    const addDiagram = (diagram) => {
      const newFigure = {
        id: state.figures.length + 1,
        type: diagram,
        color: "#aaa",
        border: "black",
      };

      switch (diagram) {
        case "vertical_line":
          newFigure.x1 = 0;
          newFigure.y1 = 0;
          newFigure.x2 = 100;
          newFigure.y2 = 100;
          break;
        case "horizonal_line":
          newFigure.x1 = 0;
          newFigure.y1 = 0;
          newFigure.x2 = 100;
          newFigure.y2 = 100;
          break;
        case "if":
        case "function":
        case "start":
        case "end":
        case "loop_start":
        case "loop_end":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break;
      }
      state.figures.push(newFigure);
    };
    return {
      state,
      addDiagram,
    };
  },
});
</script>
