<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <!-- <KonvaBox></KonvaBox> -->
    <v-stage
      ref="stage"
      :config="stageConfig"
      @mousemove="handleMouseMove"
      @touchmove="handleMouseMove"
      @mouseDown="handleMouseDown"
      @touchstart="handleMouseDown"
      @mouseUp="handleMouseUp"
      @touchend="handleMouseUp"
    >
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
        width: 200,
        height: 200,
      },
      configCircle: {
        x: 100,
        y: 100,
        radius: 70,
        fill: "red",
        stroke: "black",
        strokeWidth: 4,
      },
      stageConfig: {
        draggable: false,
        x: 0,
        y: 0,
        width: 500,
        height: 500,
      },
      isPaint: false,
      mode: "brush",
      lastLine: null,
    };
  },
  methods: {
    handleMouseDown() {
      const layer = this.$refs.layer.getNode();
      this.isPaint = true;
      const pos = this.$refs.stage.getNode().getPointerPosition();
      // var pos = stage.getPointerPosition();
      this.lastLine = new Konva.Line({
        stroke: "#df4b26",
        strokeWidth: 5,
        tension: 0.5,
        globalCompositeOperation:
          this.mode === "brush" ? "source-over" : "destination-out",
        points: [pos.x, pos.y],
      });
      layer.add(this.lastLine);
    },
    handleMouseMove() {
      if (!this.isPaint) {
        return;
      }

      const layer = this.$refs.layer.getNode();
      // const pos = stage.getPointerPosition();
      const pos = this.$refs.stage.getNode().getPointerPosition();

      var newPoints = this.lastLine.points().concat([pos.x, pos.y]);
      this.lastLine.points(newPoints);
      layer.batchDraw();
    },
    handleMouseUp() {
      this.isPaint = false;
    },
  },
  mounted() {
    this.stageConfig.height = window.innerHeight;
    this.stageConfig.width = window.innerWidth;
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
