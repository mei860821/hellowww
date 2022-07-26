<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <KonvaBox></KonvaBox> -->
    <div id="container"></div>

    <!-- <v-stage ref="stage" :config="configKonva">
      <v-layer ref="layer">
        <v-circle :config="configCircle"></v-circle>
      </v-layer>
    </v-stage> -->
  </div>
</template>

<script>
// import Vue from "vue";
// import VueKonva from "vue-konva";
import Konva from "konva";

// Vue.use(VueKonva);

export default {
  name: "App",
  components: {},
  data() {
    return {};
  },
  methods: {},
  mounted() {
    // this.stageConfig.height = window.innerHeight;
    // this.stageConfig.width = window.innerWidth;
    const stage = new Konva.Stage({
      container: "container",
      width: window.innerWidth,
      height: window.innerHeight - 25,
    });
    const layer = new Konva.Layer({
      listening: false,
    });
    stage.add(layer);
    // this.staggee = stage;
    // this.layer = layer;
    let isPaint = false;
    let mode = "brush";
    let lastLine;
    stage.on("mousedown touchstart", function () {
      isPaint = true;
      const pos = stage.getPointerPosition();
      lastLine = new Konva.Line({
        stroke: "#df4b26",
        strokeWidth: 5,
        tension: 0.5,
        globalCompositeOperation:
          mode === "brush" ? "source-over" : "destination-out",
        points: [pos.x, pos.y],
        perfectDrawEnabled: false,
      });
      layer.add(lastLine);
    });

    stage.on("mouseup touchend", function () {
      isPaint = false;
    });

    // and core function - drawing
    stage.on("mousemove touchmove", function () {
      if (!isPaint) {
        return;
      }

      const pos = stage.getPointerPosition();
      const newPoints = lastLine.points().concat([pos.x, pos.y]);
      lastLine.points(newPoints);
      layer.batchDraw();
    });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 500px;
  height: 500px;
}
</style>
