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
        @clearEvent="callClearResize"
        @downloadEventPNG="callDownloadPNG"
        @downloadEventJPEG="callDownloadJPEG"
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
        color: "#000000",
        type: "single"
      },
      settings: {
        size: "8",
      }
    }
  },
  methods: {
    /**
     * Function is called from Settings.vue when #sizeSelect changes or #clear-button is clicked.
     * It calls the changeCanvas function in Canvas.vue.
     */
    callClearResize(){
      this.$refs.canvas.changeCanvas()
    },
    /**
     * Function is called from Settings.vue, when #download-button for PNG is clicked.
     * It calls the downloadCanvas function in Canvas.vue and gives 'png' as image format.
     */
    callDownloadPNG(){
      this.$refs.canvas.downloadCanvas("png")
    },
    /**
     * Function is called from Settings.vue, when #download-button for JPEG is clicked.
     * It calls the downloadCanvas function in Canvas.vue and gives 'jpeg' as image format.
     */
    callDownloadJPEG(){
      this.$refs.canvas.downloadCanvas("jpeg")
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
