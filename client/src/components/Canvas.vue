<template>
  <div id="container">
    <div id="square-container">
      <div v-for="(row, indexRow) in colorMatrix" :key="indexRow">
        <div
            v-for="(entry, indexColumn) in row"
            :key="indexColumn"
            :id="'square'+ indexRow + '-' + indexColumn"
            class="square" @click="changeColor(indexRow, indexColumn)">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import domtoimage from "dom-to-image";

export default {
  name: "Canvas",
  props: ["inputPen", "inputSettings"],
  data() {
    return {
      colorMatrix: [
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"],
        ["#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF", "#FFFFFF"]
      ],
      pen: this.inputPen,
      settings: this.inputSettings
    }
  },
  methods: {
    /**
     * After click on .square this function is called.
     * It sets the color in the colorMatrix for that square. After that this squares css background color is set to
     * the same color.
     *
     * @param indexRow  the vertical position of the square in the matrix
     * @param indexColumn  the horizontal position of the square in the matrix
     */
    changeColor(indexRow, indexColumn) {
      this.colorMatrix[indexRow][indexColumn] = this.pen.color
      let id = "square" + indexRow + "-" + indexColumn
      document.getElementById(id).style.background = this.colorMatrix[indexRow][indexColumn]
    },
    /**
     * Function takes all squares and sets there css background color to the belonging color in colorMatrix.
     */
    paintAllSquares() {
      for (let i in this.colorMatrix) {
        for (let j in this.colorMatrix[i]) {
          let id = "square" + i + "-" + j
          document.getElementById(id).style.background = this.colorMatrix[i][j]
        }
      }
    },
    /**
     * Function recreates the colorMatrix with the size the user has chosen and fills it with hex codes for the color
     * white.
     */
    createSquares() {
      this.colorMatrix = []
      let length = this.settings.size;
      for (let i = 0; i < length; i++) {
        let tempArray = []
        for (let j = 0; j < length; j++) {
          tempArray.push("#FFFFFF")
        }
        this.colorMatrix.push(tempArray)
      }
    },
    /**
     * Function calculates and sets the height and width of every .square.
     */
    resizeSquares() {
      let pixel = 800 / this.settings.size - 2;
      let elements = document.getElementsByClassName("square")
      for (let element of elements){
        element.style.width = pixel + "px"
        element.style.height = pixel + "px"
      }

    },
    /**
     * Function is called from Settings.vue via #settings-button.
     * It calls the functions that are needed to set the square numbers and attributes.
     */
    changeCanvas() {
      this.createSquares()
      this.$nextTick(() => this.paintAllSquares())
      this.$nextTick(() => this.resizeSquares())
    },
    /**
     * Function is called from Settings.vue via #download-button.
     * It takes #square-container and turns it to a blob. Then it gives the user the possibility to download it.
     * For turning the HTML-component into a blob the "dom-to-image" library is used. For the download the
     * "file-saver" library is used.
     */
    async downloadCanvas() {
      let element = document.getElementById('square-container')
      element.style.margin = "0"
      await domtoimage.toBlob(element)
        .then(function(blob) {
          let FileSaver = require('file-saver')
          console.log(blob)
          FileSaver.saveAs(blob, "output.jpeg")
      })
      element.style.margin = "0 auto";
    },
  },
  /**
   * Function is called after the HTML is created.
   * It uses css to set the color of the canvas squares to the colorMatrix colors.
   */
  mounted() {
    for (let i in this.colorMatrix) {
      for (let j in this.colorMatrix[i]) {
        let id = "square" + i + "-" + j
        document.getElementById(id).style.background = this.colorMatrix[i][j]
      }
    }
  }
}
</script>

<style scoped>
#container {
  width: 100%;
}
#square-container {
  background-color: black;
  margin: 0 auto;
  min-width: 800px;
  max-width: 800px;
  height: 800px;
  border: 1px solid black;
}
.square{
  width: 98px;
  height: 98px;
  background: white;
  border: 1px solid black;
  float: left;
/*  width: calc(12.5% - 2px);
  padding-top: calc(12.5% - 2px);
  background: white;
  border: 1px solid black;
  float: left;*/
}
</style>