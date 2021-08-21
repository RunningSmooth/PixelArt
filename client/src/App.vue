<template>
  <h1>PixelArt</h1>
    <ColorPalette
        style="width: 10%; float: left"
        :changeColor="(inputColor) => changeColor(inputColor)"
    />
    <Canvas
        style="width: calc(80% - 4px); float: left"
        :inputPen="pen"
    />
    <Settings style="width: 10%; float: left"/>
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
      }
    }
  },
  methods: {
    changeColor(inputColor) {
      this.pen.color = inputColor
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
