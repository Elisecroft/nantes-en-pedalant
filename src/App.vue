<template>
  <div id="app">
    <SideNavbar />
    <Map />
  </div>
</template>

<script>
import Map from './components/Map.vue'
import SideNavbar from './components/SideNavbar.vue';

export default {
  name: 'app',
  components: {
    Map,
    SideNavbar,
  },
}
</script>

<style>
  body {
    margin: 0;
    padding: 0;
  }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
</style>