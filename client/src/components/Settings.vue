<template>
  <div id="settings-container">
    <h3>Settings</h3>
    <br>
    <div id="size-container">
      <label for="sizeSelect">Feldgröße:
        <select class="dropDown" id="sizeSelect" v-model="settings.size">
          <option value="8">8x8</option>
          <option value="12">12x12</option>
          <option value="16">16x16</option>
          <option value="32">32x32</option>
        </select>
      </label>
    </div>
    <br>
    <button id="resize-button" @click="$emit('resizeEvent')">Resize</button>
    <br>
    <button id="download-button" @click="$emit('downloadEvent')"><fa icon="download"/></button>

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
  background: lightgrey;
  top: 120px;
  height: 400px;
  border: 1px solid black;
}
#size-container{
  font-size: 20px;
}
.dropDown{
  font-size: 20px;
  color: #2c3e50;
  border-color: #2c3e50;
}
button {
  font-size: 20px;
  border-radius: 6px;
}
button:hover{
  background: gray;
}
#download-button {

}
</style>