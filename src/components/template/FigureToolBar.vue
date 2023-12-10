<template>
  <v-container class="mt-10 mb-10">
    <v-card class="d-inline-flex" max-width="1000">
    <v-row class="d-flex justify-center overflow-x-auto pa-10">
      <v-col v-for="option in buttonOptions" :key="option" class="ml-2 mr-2">
        <v-card elevation="10" class="pa-2 rounded-lg" min-width="100">
          <v-btn
            text
            width="100%"
            height="100%"
            class="text-h6"
            @click="onClick(option.object)"
            >{{ option.label }}</v-btn
          >
        </v-card>
      </v-col>
    </v-row>
    </v-card>
  </v-container>
</template>
<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "FigureToolBar",
  props: {},
  emits: ["click"],
  setup(props, { emit }) {
    const buttonOptions = [
      {
        label: "|",
        object: "vertical_line",
      },
      {
        label: "―",
        object: "horizonal_line",
      },
      {
        label: "IF",
        object: "if"
      },
      {
        label: "PROCESS",
        object: "process"
      },
      {
        label: "FUNCTION",
        object: "function"
      },
      {
        label: "START",
        object: "start"
      },
      {
        label: "END",
        object: "end"
      },
      {
        label: "LOOP_START",
        object: "loop_start"
      },
      {
        label: "LOOP_END",
        object: "loop_end"
      },
      {
        label: "ASSIGN",
        object: "assign"
      },
    ];
    const onClick = (object) => {
      emit("click", object);
    };
    return {
      buttonOptions,
      onClick,
    };
  },
});
</script>
