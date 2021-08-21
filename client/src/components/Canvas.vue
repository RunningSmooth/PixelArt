<template>
  <div id="container">
    <div id="square-container">
      <div v-for="(row, indexRow) in colorArray" :key="indexRow">
        <div
            v-for="(entry, indexColumn) in row"
            :key="indexColumn"
            :id="'test'+ indexRow + '-' + indexColumn"
            class="square" @click="changeColor(indexRow, indexColumn)">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Canvas",
  props: ["inputPen"],
  data() {
    return {
      colorArray: [
        ["#2c3e50", "#2c3e50", "#2c3e50", "#2c3e50"],
        ["#2c3e50", "#2c3e50", "#2c3e50", "#2c3e50"],
        ["#2c3e50", "#254640", "#2c3e50", "#2c3e50"],
        ["#2c3e50", "#2c3e50", "#2c3e50", "#2c3e50"]
      ],
      pen: this.inputPen
    }
  },
  methods: {
    changeColor(indexRow, indexColumn) {
      this.colorArray[indexRow][indexColumn] = this.pen.color
      let id = "test" + indexRow + "-" + indexColumn
      document.getElementById(id).style.background = this.colorArray[indexRow][indexColumn]
    }
  },
  mounted() {
    for (let i in this.colorArray) {
      for (let j in this.colorArray[i]) {
        let id = "test" + i + "-" + j
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
  min-width: 400px;
  max-width: 400px;
  height: 400px;
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