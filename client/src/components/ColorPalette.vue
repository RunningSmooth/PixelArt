<template>
  <div id="palette-container">
    <div id="chosen-container">
      <div
           id="blob-chosen"
           class="palette-square">
      </div>
    </div>
    <hr>
    <div id="blob-container">
      <div v-for="(colorCode, index) in colors"
           :key="index"
           :id="'blob'+ index"
           class="palette-square"
           @click="setPenColor(colorCode)">
      </div>
    </div>
    <hr>
    <div id="pen-container">
      <div id="single-pen-button" class="pen-button" @click="setPenType('single')">
        Single
      </div>
      <div id="fill-pen-button" class="pen-button" @click="setPenType('fill')">
        Fill
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
  methods: {
    /**
     * After click on a color from the palette, this function is called.
     * It sets the pens color to the chosen color and changes the color of #blob-chosen to the same color.
     *
     * @param color  string with the hex code of the chosen color.
     */
    setPenColor(color) {
      this.pen.color = color
      document.getElementById('blob-chosen').style.background = color
    },
    /**
     * After click on a .pen-button this function is called.
     * It sets the pens type to the chosen type.
     *
     * @param type  string with the chosen type. It can be 'single' for single square coloring or 'fill' for filling a
     *              a whole surface with color.
     */
    setPenType(type) {
      console.log(type)
      this.pen.type = type
      switch (type){
        case "single":
          document.getElementById("single-pen-button").style.background = "white"
          document.getElementById("fill-pen-button").style.background = "transparent"
          break
        case "fill":
          document.getElementById("single-pen-button").style.background = "transparent"
          document.getElementById("fill-pen-button").style.background = "white"
          break
      }

    }
  },
  /**
   * Function is called after the HTML is created.
   * It uses css to set the color of the color palette squares to the color arrays colors. Additionally it sets the
   * color of #blob-chosen to the pens color.
   */
  mounted() {
    for (let i in this.colors) {
      let id = "blob" + i
      document.getElementById(id).style.background = this.colors[i]
    }
    document.getElementById('blob-chosen').style.background = this.pen.color
    document.getElementById(this.pen.type + "-pen-button").style.background = "white"
  }
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
#chosen-container{
  width: 100%;
  text-align: center;
  padding-top: 10px;
  padding-bottom: 10px;
}
#blob-chosen{
  margin: 0 auto;
  display: inline-block;
  float: none;
}
#blob-chosen:hover{
  filter: brightness(1);
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
  border-radius: 20px;
}
.palette-square:hover{
  filter: brightness(0.50);
}
#pen-container{
  width: 100%;
  text-align: center;
  padding-top: 10px;
  padding-bottom: 10px;
}
.pen-button{
  width: calc(30% - 2px);
  padding-top: calc(20% - 2px);
  margin-left: 10%;
  margin-right: 10%;
  margin-bottom: 10px;
  position: relative;
  border: 1px solid black;
  float: left;
  border-radius: 5px;
}
.pen-button:hover{
  background: gray;
}
</style>