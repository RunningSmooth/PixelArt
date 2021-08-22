<template>
  <h1>PixelArt</h1>
    <ColorPalette
        style="width: 15%; float: left"
        :inputPen="pen"
    />
    <Canvas
        style="width: calc(70% - 4px); float: left"
        :inputPen="pen"
        :inputSettings="settings"
        ref="canvas"
    />
    <Settings
        style="width: 15%; float: left"
        :inputSettings="settings"
        @resizeEvent="callResize"
        @downloadEvent="callDownload"
    />
</template>

<script>
import Canvas from "@/components/Canvas";
import ColorPalette from "@/components/ColorPalette";
import Settings from "@/components/Settings";

export default {
  name: 'App',
  components: {
    Canvas, Settings, ColorPalette
  },
  data() {
    return {
      pen: {
        color: "#000000"
      },
      settings: {
        size: "8"
      }
    }
  },
  methods: {
    /**
     * Function is called from Settings.vue, when #resize-button is clicked.
     * It calls the changeCanvas function in Canvas.vue.
     */
    callResize(){
      this.$refs.canvas.changeCanvas()
    },
    /**
     * Function is called from Settings.vue, when #download-button is clicked.
     * It calls the downloadCanvas function in Canvas.vue.
     */
    callDownload(){
      this.$refs.canvas.downloadCanvas()
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
