<template>
  <div id="app"><CanvasComponent /></div>
</template>

<script>
import CanvasComponent from "./components/CanvasComponent";

export default {
  name: "App",
  components: {
    CanvasComponent
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #201d35;
}
</style>
