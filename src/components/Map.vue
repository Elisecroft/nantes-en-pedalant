<template>
  <div style="height: 100vh; width: 100vw">
    <LMap
      style="height: 100%; width: 100%"
      :zoom="zoom"
      :center="center"
      @update:zoom="zoomUpdated"
      @update:center="centerUpdated"
      @update:bounds="boundsUpdated"
    >
      <LTileLayer :url="url"></LTileLayer>
      <LMarker v-for="pompe in pompes":lat-lng="pompe.geometry.coordinates">
        <LPopup :content="pompe.fields.descriptif"></LPopup>
      </LMarker>
    </LMap>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from "vue2-leaflet";
import { Icon } from "leaflet"; // Popup icon
import axios from "axios";
import Vue from "vue";
import "leaflet/dist/leaflet.css";

Vue.component("l-map", LMap);
Vue.component("l-tile-layer", LTileLayer);
Vue.component("l-marker", LMarker);
Vue.component("l-popup", LPopup);

// Icon part
delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png")
});

export default {
  data() {
    return {
      url:
        "https://api.mapbox.com/styles/v1/mapbox/streets-v11/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiZWxpc2Vjcm9mdCIsImEiOiJjazRjY2FxamowNDN6M21vMXBpaGV1bXNhIn0.ZlAf35jELFpyEr3pR0Lx1A",
      zoom: 13,
      center: [47.213039, -1.549931], //GMap
      markers: [{
        latlng: [47.216303, -1.550231],
        content: "hello"
      },
      {
        latlng: [46.216303, -1.350231],
        content: "hola"
      }],
      pompes: null 
    };
  },
  mounted() {
    const urlProxy = "https://cors-anywhere.herokuapp.com/";
    const dataPompes = "https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_gonfleurs-libre-service-nantes-metropole&rows=32";
    axios
      .get(dataPompes)
      .then(res => {
        this.pompes = res.data.records;
        for (let i = 0; i < this.pompes.length; i++) {
          let newCoord = this.pompes[i].geometry.coordinates.reverse();
          console.log(newCoord)
          this.pompes[i].geometry.coordinates = newCoord;
        }
      })
  },
  methods: {
    zoomUpdated(zoom) {
      this.zoom = zoom;
    },
    centerUpdated(center) {
      this.center = center;
    },
    boundsUpdated(bounds) {
      this.bounds = bounds;
    }
  },
  name: "Map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  }
};
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>