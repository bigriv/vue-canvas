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
            <template
              v-for="i in state.field.width / state.field.interval - 1"
              :key="i"
            >
              <line
                :x1="i * 100"
                :y1="0"
                :x2="i * 100"
                :y2="state.field.height"
                fill="black"
                stroke="black"
                stroke-dasharray="4 4"
              />
            </template>
            <Diagrams :figures="figures" @grab="onGrab" @release="onRelease" />
          </svg>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { defineComponent, reactive } from "vue";

export default defineComponent({
  name: "GridCanvas",
  components: {},
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
        interval: 100,
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
    return {
      state,
      onEscape,
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
