<template>
  <FigureToolBar @click="addObject" />
  <v-container
    tabindex="0"
    align="center"
    class="none-focus-border"
    @keydown.esc="onEscape"
  >
    <v-row>
      <v-col>
        <v-card elevation="10" :max-width="state.field.width">
          <svg
            :viewbox="`0 0 ${state.field.width} ${state.field.height}`"
            :width="state.field.width"
            :height="state.field.height"
            @mouseleave="state.isEnterCanvas = false"
            @mouseenter="state.isEnterCanvas = true"
            @mousemove="onMousemove"
          >
            <template v-for="figure in state.figures" :key="figure">
              <BasicCircle
                v-if="figure.type === 'circle'"
                v-model:r="figure.r"
                v-model:x="figure.x"
                v-model:y="figure.y"
                :color="figure.color"
                :border="figure.border"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <BasicRect
                v-else-if="figure.type === 'rect'"
                v-model:x="figure.x"
                v-model:y="figure.y"
                v-model:width="figure.width"
                v-model:height="figure.height"
                :color="figure.color"
                :border="figure.border"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <BasicLine
                v-else-if="figure.type === 'line'"
                v-model:x1="figure.x1"
                v-model:y1="figure.y1"
                v-model:x2="figure.x2"
                v-model:y2="figure.y2"
                :color="figure.color"
                :border="figure.border"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <BasicPolygon
                v-else-if="figure.type === 'polygon'"
                v-model:points="figure.points"
                :color="figure.color"
                :border="figure.border"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <ConditionalDiagram
                v-else-if="figure.type === 'if'"
                v-model:x="figure.x"
                v-model:y="figure.y"
                v-model:width="figure.width"
                v-model:height="figure.height"
                :color="figure.color"
                :border="figure.border"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <FunctionDiagram
                v-else-if="figure.type === 'function'"
                v-model:x="figure.x"
                v-model:y="figure.y"
                v-model:width="figure.width"
                v-model:height="figure.height"
                :color="figure.color"
                :border="figure.border"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <StartAndEndDiagram
                v-else-if="figure.type === 'start'"
                v-model:x="figure.x"
                v-model:y="figure.y"
                v-model:width="figure.width"
                v-model:height="figure.height"
                :type="figure.type"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
              <StartAndEndDiagram
                v-else-if="figure.type === 'end'"
                v-model:x="figure.x"
                v-model:y="figure.y"
                v-model:width="figure.width"
                v-model:height="figure.height"
                :type="figure.type"
                @grab="onGrab(figure)"
                @release="onRelease(figure)"
              />
            </template>
          </svg>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { defineComponent, reactive } from "vue";
import BasicCircle from "@/components/atoms/BasicCircle.vue";
import BasicRect from "@/components/atoms/BasicRect.vue";
import BasicLine from "@/components/atoms/BasicLine.vue";
import BasicPolygon from "@/components/atoms/BasicPolygon.vue";
import ConditionalDiagram from "@/components/molecules/ConditionalDiagram.vue";
import FunctionDiagram from "@/components/molecules/FunctionDiagram.vue";
import StartAndEndDiagram from "@/components/molecules/StartAndEndDiagram.vue";
import FigureToolBar from "@/components/molecules/FigureToolBar.vue";

export default defineComponent({
  name: "Canvas",
  components: {
    BasicCircle,
    BasicRect,
    BasicLine,
    BasicPolygon,
    ConditionalDiagram,
    FunctionDiagram,
    StartAndEndDiagram,
    FigureToolBar,
  },
  setup() {
    const state = reactive({
      field: {
        width: 1000,
        height: 800,
      },
      figures: [],
      grabList: [],
      isEnterCanvas: false,
    });
    const onEscape = () => {
      state.grabList.splice(0);
    };
    const addObject = (object) => {
      const newFigure = {
        id: state.figures.length + 1,
        type: object,
        color: "#aaa",
        border: "black",
      };

      switch (object) {
        case "circle":
          newFigure.r = 20;
          newFigure.x = 20;
          newFigure.y = 20;
          break;
        case "rect":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break;
        case "line":
          newFigure.x1 = 0;
          newFigure.y1 = 0;
          newFigure.x2 = 100;
          newFigure.y2 = 100;
          break;
        case "polygon":
          newFigure.points = [
            [0, 50],
            [100, 0],
            [200, 50],
            [100, 100],
          ];
          break;
        case "if":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break
        case "function":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break;
        case "start":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break
        case "end":
          newFigure.x = 0;
          newFigure.y = 0;
          newFigure.width = 100;
          newFigure.height = 40;
          break
      }
      state.figures.push(newFigure);
    };
    const onGrab = (figure) => {
      state.grabList.push(figure);
    };
    const onRelease = (figure) => {
      state.grabList = state.grabList.filter((item) => item.id !== figure.id);
    };
    const onMousemove = (event) => {
      state.grabList.forEach((item) => {
        switch (item.type) {
          case "circle":
          case "rect":
            item.x += event.movementX * 0.7;
            item.y += event.movementY * 0.7;
            break;
          case "line":
            item.x1 += event.movementX * 0.7;
            item.y1 += event.movementY * 0.7;
            item.x2 += event.movementX * 0.7;
            item.y2 += event.movementY * 0.7;
            break;
          case "polygon":
            item.points.forEach((point) => {
              point[0] += event.movementX * 0.7;
              point[1] += event.movementY * 0.7;
            });
            break;            
        }
      });
    };
    return {
      state,
      onEscape,
      addObject,
      onMousemove,
      onGrab,
      onRelease,
    };
  },
});
</script>
<style scoped>
.none-focus-border:focus {
  outline: none;
}
</style>
