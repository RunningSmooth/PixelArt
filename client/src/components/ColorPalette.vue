<template>
  <div id="palette-container">
    <div id="blob-container">
      <div v-for="(colorCode, index) in colors"
           :key="index"
           :id="'blob'+ index"
           class="palette-square"
           @click="pickColor(colorCode)">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ColorPalette",
  props: ["changeColor"],
  data() {
    return {
      colors: ["#FF0000", "#0000FF", "#FFFF00", "#008000", "#FFFFFF", "#000000"],
      pickedColor: ""
    }
  },
  mounted() {
    for (let i in this.colors) {
      let id = "blob" + i
      document.getElementById(id).style.background = this.colors[i]
    }
  },
  methods: {
    pickColor(color) {
      this.pickedColor = color
      this.changeColor(color)
    }
  },
  created() {
/*    this.setSquareHeight()*/
  }
}
</script>

<style scoped>
#palette-container{
  background: lightgrey;
  top: 120px;
  height: 400px;
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
  width: 35%;
  padding-top: 35%;
  margin-left: 5px;
  margin-right: 5px;
  margin-bottom: 10px;
  position: relative;
  border: 1px solid black;
  float: left;
}
</style>