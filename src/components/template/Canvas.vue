<template>
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
            <Diagrams :figures="figures" @grab="onGrab" @release="onRelease" />
          </svg>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { defineComponent, reactive } from "vue";
import Diagrams from "@/components/organisms/Diagrams.vue";

export default defineComponent({
  name: "Canvas",
  components: {
    Diagrams,
  },
  props: {
    figures: {
      type: Array,
      default: () => [],
    },
  },
  setup() {
    const state = reactive({
      field: {
        width: 1000,
        height: 800,
      },
      grabList: [],
      isEnterCanvas: false,
    });
    const onEscape = () => {
      state.grabList.splice(0);
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
