<template>
  <div id="container">
    <div id="square-container">
      <div v-for="(row, indexRow) in colorArray" :key="indexRow">
        <div
            v-for="(entry, indexColumn) in row"
            :key="indexColumn"
            :id="'square'+ indexRow + '-' + indexColumn"
            class="square" @click="changeColor(indexRow, indexColumn)">
        </div>
      </div>
    </div>
    <button @click="changeCanvas()">Resize</button>
    <p>{{settings}}</p>
  </div>
</template>

<script>
export default {
  name: "Canvas",
  props: ["inputPen", "inputSettings"],
  data() {
    return {
      colorArray: [
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
    changeColor(indexRow, indexColumn) {
      this.colorArray[indexRow][indexColumn] = this.pen.color
      let id = "square" + indexRow + "-" + indexColumn
      document.getElementById(id).style.background = this.colorArray[indexRow][indexColumn]
    },
    paintAllSquares() {
      for (let i in this.colorArray) {
        for (let j in this.colorArray[i]) {
          let id = "square" + i + "-" + j
          document.getElementById(id).style.background = this.colorArray[i][j]
        }
      }
    },
    createSquares() {
      this.colorArray = []
      let length = this.settings.size;
      for (let i = 0; i < length; i++) {
        let tempArray = []
        for (let j = 0; j < length; j++) {
          tempArray.push("#FFFFFF")
        }
        this.colorArray.push(tempArray)
      }
    },
    resizeSquares() {
      let pixel = 800 / this.settings.size - 2;
      let elements = document.getElementsByClassName("square")
      for (let element of elements){
        element.style.width = pixel + "px"
        element.style.height = pixel + "px"
      }

    },
    changeCanvas() {
      this.createSquares()
      this.$nextTick(() => this.paintAllSquares())
      this.$nextTick(() => this.resizeSquares())

    }

  },
  mounted() {
    for (let i in this.colorArray) {
      for (let j in this.colorArray[i]) {
        let id = "square" + i + "-" + j
        document.getElementById(id).style.background = this.colorArray[i][j]
      }
    }
  },
  created() {

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
}
</style>