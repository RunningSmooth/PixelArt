<template>
  <div id="settings-container">
    <h2>Settings</h2>
    <div id="size-container">
      <label for="sizeSelect">Size:
        <select class="dropDown" id="sizeSelect" v-model="settings.size" @change="$emit('clearEvent')">
          <option value="8">8x8</option>
          <option value="12">12x12</option>
          <option value="16">16x16</option>
          <option value="32">32x32</option>
        </select>
      </label>
    </div>
    <div id="clear-button" @click="$emit('clearEvent')">
      <fa icon="trash-alt"/>
    </div>
    <br>
    <hr>
    <h2>Download</h2>
    <div class="download-button" @click="$emit('downloadEventJPEG')">
      <span>JPEG</span><br>
      <fa icon="download"/>
    </div>
    <div class="download-button" @click="$emit('downloadEventPNG')">
      <span>PNG</span><br>
      <fa icon="download"/>
    </div>
  </div>
</template>

<script>
export default {
  name: "Settings",
  props: ["inputSettings"],
  data() {
    return {
      settings: this.inputSettings
    }
  }
}
</script>

<style scoped>
#settings-container{
  min-width: 140px;
  background: lightgrey;
  top: 120px;
  border: 1px solid black;
}
#size-container{
  font-size: 20px;
}
.dropDown{
  font-size: 20px;
  color: #2c3e50;
  border-color: #2c3e50;
  cursor: pointer;
}
#clear-button{
  background: white;
  width: calc(30% - 2px);
  padding-top: 4px;
  margin: 20px 10% 10px;
  position: relative;
  border: 1px solid black;
  display: inline-block;
  border-radius: 5px;
  font-size: 2em;
  cursor: pointer;
}
#clear-button:hover{
  filter: brightness(0.70);
}
.download-button{
  background: white;
  width: calc(30% - 2px);
  margin-left: 10%;
  margin-right: 10%;
  margin-bottom: 10px;
  padding-bottom: 5px;
  position: relative;
  border: 1px solid black;
  float: left;
  border-radius: 5px;
  font-size: 2em;
  cursor: pointer;
}
.download-button:hover{
  filter: brightness(0.70);
}
span{
  font-size: 20px;
}
</style>