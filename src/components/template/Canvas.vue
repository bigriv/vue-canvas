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
          <div style="position: relative" id="input-area" />
          <svg
            :viewbox="`0 0 ${state.field.width} ${state.field.height}`"
            :width="state.field.width"
            :height="state.field.height"
            @mouseleave="state.isEnterCanvas = false"
            @mouseenter="state.isEnterCanvas = true"
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
