<template>
  <div id="app">
    <HereMap
      ref="map"
      apiKey=""
      lat="47.2205"
      lng="-1.5600"
      zoom="13" />
  </div>
</template>

<script>
  let proxySettings = {
    proxyType: "system",
    http: "http://pubproxy.com/api/proxy?api=123",
    passthrough: "127.0.0.1"
  };

  browser.proxy.settings.set({value: proxySettings});

  import HereMap from "./components/HereMap.vue"

  export default {
    name: 'app',
    components: {
      HereMap
    },
    mounted() {
      let map = this.$refs.map;
      map.drawRoute(
        {
          lat: "48",
          lng: "-1.4"
        },
        {
          lat: "47.8",
          lng: "-1.3"
        }
      )
    } 
  }
</script>

<style>
  body {
    margin: 0;
  }
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
</style>