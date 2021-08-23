<template>
  <div id="container">
    <div id="square-container">
      <div v-for="(row, indexRow) in colorMatrix" :key="indexRow">
        <div
            v-for="(entry, indexColumn) in row"
            :key="indexColumn"
            :id="'square'+ indexRow + '-' + indexColumn"
            class="square"
            @click="changeColor(indexRow, indexColumn)">
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
      settings: this.inputSettings,
      checkMatrix: []
    }
  },
  methods: {
    changeColor(indexRow, indexColumn) {
      if (this.pen.type === "single") {
        this.changeSingleColor(indexRow, indexColumn);
      } else if (this.pen.type === "fill") {
        this.changeFillColor(indexRow, indexColumn);
      }
    },
    /**
     * Function is called by changeColor(iR, iC) when the pen type is 'single' and by changeFillColor(iR, iC).
     * It sets the color in the colorMatrix for that square. After that this squares css background color is set to
     * the same color.
     *
     * @param indexRow  the vertical position of the square in the matrix
     * @param indexColumn  the horizontal position of the square in the matrix
     */
    changeSingleColor(indexRow, indexColumn) {
      this.colorMatrix[indexRow][indexColumn] = this.pen.color;
      let id = "square" + indexRow + "-" + indexColumn;
      document.getElementById(id).style.background = this.colorMatrix[indexRow][indexColumn];
    },
    /**
     * Function is called by changeColor(iR, iC) when the pen type is 'fill'.
     * It sets up a checkMatrix and calls a recursive-like function that checks which adjacent squares have the same
     * color. After the function finishes the chosen squares are colored.
     *
     * @param indexRow  the vertical position of the square in the matrix
     * @param indexColumn  the horizontal position of the square in the matrix
     */
    changeFillColor(indexRow, indexColumn) {
      // Set up an 'empty' checkMatrix. Every entry belongs to an entry of colorMatrix. The possible entry-values are
      // 0 (not checked), 1 (checked and same color as start square) and -1 (checked and not same color as start square)
      this.checkMatrix = []
      let length = this.settings.size;
      for (let i = 0; i < length; i++) {
        let tempArray = [];
        for (let j = 0; j < length; j++) {
          tempArray.push(0);
        }
        this.checkMatrix.push(tempArray);
      }
      this.checkMatrix[indexRow][indexColumn] = 1
      // Start recursive search for same colored adjacent squares.
      this.recursiveColorMatching(indexRow, indexColumn)
      // Check the checkMatrix for every entry that equals 1 and call function to color the belonging square.
      for (let i = 0; i<this.settings.size ;i++) {
        for (let j = 0; j<this.settings.size ;j++) {
          if (this.checkMatrix[i][j] === 1) {
            this.changeSingleColor(i, j)
          }
        }
      }
    },
    /**
     * Function is called by changeFillColor(iR, iC).
     * It checks if the color of the squares that are adjacent to the one with the input coordinates are matching the
     * the color of the input square. It only checks an adjacent square if it is entry in checkMatrix is 0. Else it
     * was checked. If the color matches the adjacent squares entry in checkMatrix is set to 1 and the function is
     * called with the adjacent square as input. Else the adjacent squares entry in checkMatrix is set to -1.
     *  color. After the function finishes the chosen squares are colored.
     *
     * @param indexRow  the vertical position of the square in the matrix
     * @param indexColumn  the horizontal position of the square in the matrix
     */
    recursiveColorMatching(indexRow, indexColumn){
      // Check square which is positioned under the input square
      if (indexRow < this.settings.size - 1) {
        if (this.checkMatrix[indexRow+1][indexColumn] === 0) {
          if (this.colorMatrix[indexRow+1][indexColumn] === this.colorMatrix[indexRow][indexColumn]){
            this.checkMatrix[indexRow+1][indexColumn] = 1
            this.recursiveColorMatching(indexRow+1, indexColumn)
          } else {
            this.checkMatrix[indexRow+1][indexColumn] = -1
          }
        }
      }
      // Check square which is positioned over the input square
      if (indexRow > 0) {
        if (this.checkMatrix[indexRow-1][indexColumn] === 0) {
          if (this.colorMatrix[indexRow-1][indexColumn] === this.colorMatrix[indexRow][indexColumn]){
            this.checkMatrix[indexRow-1][indexColumn] = 1
            this.recursiveColorMatching(indexRow-1, indexColumn)
          } else {
            this.checkMatrix[indexRow-1][indexColumn] = -1
          }
        }
      }
      // Check square which is positioned right of input square
      if (indexColumn < this.settings.size - 1) {
        if (this.checkMatrix[indexRow][indexColumn+1] === 0){
          if (this.colorMatrix[indexRow][indexColumn+1] === this.colorMatrix[indexRow][indexColumn]){
            this.checkMatrix[indexRow][indexColumn+1] = 1
            this.recursiveColorMatching(indexRow, indexColumn+1)
          } else {
            this.checkMatrix[indexRow][indexColumn+1] = -1
          }
        }
      }
      // Check square which is positioned left of the input square
      if (indexColumn > 0) {
        if (this.checkMatrix[indexRow][indexColumn-1] === 0){
          if (this.colorMatrix[indexRow][indexColumn-1] === this.colorMatrix[indexRow][indexColumn]){
            this.checkMatrix[indexRow][indexColumn-1] = 1
            this.recursiveColorMatching(indexRow, indexColumn-1)
          } else {
            this.checkMatrix[indexRow][indexColumn-1] = -1
          }
        }
      }
    },
    /**
     * Function takes all squares and sets there css background color to the belonging color in colorMatrix.
     */
    paintAllSquares() {
      for (let i in this.colorMatrix) {
        for (let j in this.colorMatrix[i]) {
          let id = "square" + i + "-" + j;
          document.getElementById(id).style.background = this.colorMatrix[i][j];
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
        let tempArray = [];
        for (let j = 0; j < length; j++) {
          tempArray.push("#FFFFFF");
        }
        this.colorMatrix.push(tempArray);
      }
    },
    /**
     * Function calculates and sets the height and width of every .square.
     */
    resizeSquares() {
      let pixel = 800 / this.settings.size - 2;
      let elements = document.getElementsByClassName("square")
      for (let element of elements){
        element.style.width = pixel + "px";
        element.style.height = pixel + "px";
      }
    },
    /**
     * Function is called from Settings.vue when #sizeSelect changes or when #clear-button is clicked.
     * It calls the functions that are needed to set the square numbers and attributes.
     */
    changeCanvas() {
      this.createSquares();
      this.$nextTick(() => this.paintAllSquares());
      this.$nextTick(() => this.resizeSquares());
    },
    /**
     * Function is called from Settings.vue via #download-button.
     * It takes #square-container and turns it to a blob. Then it gives the user the possibility to download it.
     * For turning the HTML-component into a blob the "dom-to-image" library is used. For the download the
     * "file-saver" library is used.
     *
     * @param format  string with the image format the user wants
     */
    async downloadCanvas(format) {
      let element = document.getElementById('square-container');
      element.style.margin = "0";
      await domtoimage.toBlob(element)
        .then(function(blob) {
          let FileSaver = require('file-saver');
          console.log(blob);
          FileSaver.saveAs(blob, "output." + format);
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
        let id = "square" + i + "-" + j;
        document.getElementById(id).style.background = this.colorMatrix[i][j];
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
  cursor: pointer;
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