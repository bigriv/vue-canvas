<template>
  <div class="mb-10" width="100%">
    <FigureToolBar @click="addDiagram" />
    <Canvas :figures="state.figures" />
    <!-- <GridCanvas :figures="state.figures" /> -->
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
        case "horizonal_line":
          newFigure.x = 10;
          newFigure.y = 10;
          newFigure.length = 100;
          break;
        case "if":
        case "process":
        case "function":
        case "start":
        case "end":
        case "loop_start":
        case "loop_end":
        case "assign":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break;
        case "text":
          newFigure.x = 100;
          newFigure.y = 100;
          newFigure.text = 'aaaa';
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
