<template>
  <Canvas />
</template>
<script>
import { defineComponent } from "vue";
import Canvas from "@/components/organisms/Canvas.vue";

export default defineComponent({
  name: "Main",
  components: {
    Canvas
  },
  setup() {},
});
</script>
