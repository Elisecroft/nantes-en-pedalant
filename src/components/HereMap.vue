<template>
  <div class="here-map">
    <div ref="map" style="width: 100w; height: 100vh"></div>
  </div>
</template>

<script>
  export default {
    name: "HereMap",
    data() {
      return {
        map: {},
        platform: {},
        routingService: {}
      }
    },
    props: {
      apiKey: String,
      lat: String,
      lng: String,
      zoom: String
    },
    created() {
      this.platform = new H.service.Platform({
        "apiKey": this.apiKey
      });
      this.routingService = this.platform.getRoutingService();
    },
    mounted() {
      let defaultLayers = this.platform.createDefaultLayers();
      this.map = new H.Map(
        this.$refs.map,
        defaultLayers.vector.normal.map,
        {
          zoom: this.zoom,
          center: { lat: this.lat, lng: this.lng }
        }
      );
    },
    methods: { 
      drawRoute(start, finish) {
        this.routingService.calculatedRoute(
          {
            "mode": "fastest;car;traffic:enable",
            "waypoint0": `${start.lat},${start.lng}`,
            "waypoint1": `${finish.lat},${finish.lng}`,
            "representation": "display"
          },
          data => {
            console.log(data);
          },
          error => {
            console.error(error);
          }
        )
      }
    }
  }
</script>

<style scoped></style>