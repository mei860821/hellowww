<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <KonvaBox></KonvaBox> -->
    <!-- <div id="container"></div> -->

    <v-stage ref="stage" :config="configKonva">
      <v-layer ref="layer">
        <v-circle :config="configCircle"></v-circle>
      </v-layer>
    </v-stage>
  </div>
</template>

<script>
import Vue from "vue";
import VueKonva from "vue-konva";
import Konva from "konva";

Vue.use(VueKonva);

export default {
  name: "App",
  components: {},
  data() {
    return {
      configKonva: {
        width: 500,
        height: 500,
      },
      configCircle: {
        x: 100,
        y: 100,
        radius: 70,
        fill: "red",
        stroke: "black",
        strokeWidth: 4,
      },
    };
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
    const layer = new Konva.Layer();
    stage.add(layer);
    // this.staggee = stage;
    // this.layer = layer;
    let isPaint = false;
    let mode = "brush";
    let lastLine;
    stage.on("mousedown touchstart", function () {
      isPaint = true;
      var pos = stage.getPointerPosition();
      lastLine = new Konva.Line({
        stroke: "#df4b26",
        strokeWidth: 5,
        tension: 0.5,
        globalCompositeOperation:
          mode === "brush" ? "source-over" : "destination-out",
        points: [pos.x, pos.y],
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
      var newPoints = lastLine.points().concat([pos.x, pos.y]);
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
