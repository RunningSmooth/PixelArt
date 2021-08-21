<template>
  <div id="settings-container">
    Settings
    <br>
    <label for="sizeSelect">
      <select class="dropDown" id="sizeSelect" v-model="settings.size">
        <option value="8">8x8</option>
        <option value="12">12x12</option>
        <option value="16">16x16</option>
        <option value="32">32x32</option>
      </select>
    </label>
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
  top: 120px;
  height: 400px;
  border: 1px solid black;
}
</style>