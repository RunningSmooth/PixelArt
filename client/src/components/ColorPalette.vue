<template>
  <div id="palette-container">
    <div id="blob-container">
      <div v-for="(colorCode, index) in colors"
           :key="index"
           :id="'blob'+ index"
           class="palette-square"
           @click="pickColor(colorCode)">
      </div>
      <div>
        <div>
          Fill
        </div>
        <div>
          Pen
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ColorPalette",
  props: ["inputPen"],
  data() {
    return {
      colors: [
          "#FF0000", "#0000FF", "#FFFF00", "#008000",
          "#FFFFFF", "#000000", "#808080", "#A52A2A",
          "#800080", "#FFA500"
      ],
      pen: this.inputPen
    }
  },
  /**
   * Function is called after the HTML is created.
   * It uses css to set the color of the color palette squares to the color arrays colors.
   */
  mounted() {
    for (let i in this.colors) {
      let id = "blob" + i
      document.getElementById(id).style.background = this.colors[i]
    }
  },
  methods: {
    /**
     * After click on a color from the palette, this function is called.
     * It sets the pens color to the chosen color.
     *
     * @param color  string with the hex code of the chosen color.
     */
    pickColor(color) {
      this.pen.color = color
    }
  },
}
</script>

<style scoped>
#palette-container{
  background: lightgrey;
  top: 120px;
  border: 1px solid black;
  text-align: center;
}
#blob-container{
  margin: 0 auto;
  padding-top: 10px;
  display: inline-block;
  width: 80%;
}
.palette-square{
  width: calc(36% - 2px);
  padding-top: calc(36% - 2px);
  margin-left: 7%;
  margin-right: 7%;
  margin-bottom: 10px;
  position: relative;
  border: 1px solid black;
  float: left;
}
</style>