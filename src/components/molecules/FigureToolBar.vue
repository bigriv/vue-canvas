<template>
  <v-container class="mt-10 mb-10">
    <v-row class="d-flex justify-center">
      <v-col v-for="option in buttonOptions" :key="option" class="ml-2 mr-2">
        <v-card elevation="10" class="pt-2 pb-2 rounded-lg" width="100">
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
        label: "○",
        object: "circle",
      },
      {
        label: "□",
        object: "rect",
      },
      {
        label: "\\",
        object: "line",
      },
      {
        label: "IF",
        object: "if"
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
      }
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
