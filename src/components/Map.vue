<template>
  <div style="height: 100vh; width: 100vw">
    <LMap
      style="height: 94%; width: 100%"
      :zoom="zoom"
      :center="center"
      @update:zoom="zoomUpdated"
      @update:center="centerUpdated"
      @update:bounds="boundsUpdated"
    >
      <LTileLayer :url="url"></LTileLayer>
      <LMarker v-if="filtres[0].display" v-for="pompe in pompes":lat-lng="pompe.fields.location" :icon="pompesIcon">
        <LPopup :content="'Pompes à velo'"></LPopup>
      </LMarker>
      <LMarker v-if="filtres[1].display && bicloo.fields.available_bikes > 0" v-for="bicloo in bicloos":lat-lng="bicloo.fields.position" :icon="biclooIcon">
        <LPopup :content="'Vélos disponibles : ' + bicloo.fields.available_bikes.toString()"></LPopup>
      </LMarker>
      <LMarker v-if="filtres[2].display" v-for="abri in abris" :lat-lng="abri.fields.location" :icon="abrisIcon">
        <LPopup :content="abri.fields.descriptif"></LPopup>
      </LMarker>
      <LMarker v-if="location != null" :lat-lng="location" :icon="hereIcon">
        <LPopup :content="'Vous êtes ici !'"></LPopup>
      </LMarker>
    </LMap>
    <nav>
      <h1>Nantes en pédalant</h1>
      <button v-if="filtres[1].display" v-on:click="changeFiltre('bicloos')">Bicloos dispo ✅</button>
      <button v-else v-on:click="changeFiltre('bicloos')">Bicloos dispo ❎</button>
      <button v-if="filtres[2].display" v-on:click="changeFiltre('abris')">Abris à vélo ✅</button>
      <button v-else v-on:click="changeFiltre('abris')">Abris à vélo ❎</button>
      <button v-if="filtres[0].display" v-on:click="changeFiltre('pompes')">Pompes ✅</button>
      <button v-else v-on:click="changeFiltre('pompes')">Pompes ❎</button>
    </nav>
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
      location: null,
      gettingLocation: false,
      errorStr:null,
      pompesIcon: L.icon({
        iconUrl: 'https://image.flaticon.com/icons/png/512/1493/1493719.png',
        iconSize:     [35, 45],
        shadowSize:   [50, 64],
        iconAnchor:   [18, 22],
        shadowAnchor: [4, 62],
        popupAnchor:  [-3, -76]
      }),
      biclooIcon: L.icon({
        iconUrl: 'https://image.flaticon.com/icons/png/512/1493/1493724.png',
        iconSize:     [45, 40],
        shadowSize:   [50, 64],
        iconAnchor:   [22, 20],
        shadowAnchor: [4, 62],
        popupAnchor:  [-3, -76]
      }),
      abrisIcon: L.icon({
        iconUrl: 'https://image.flaticon.com/icons/png/512/1493/1493743.png',
        iconSize:     [35, 35],
        shadowSize:   [50, 64],
        iconAnchor:   [18, 18],
        shadowAnchor: [4, 62],
        popupAnchor:  [-3, -76]
      }),
      hereIcon: L.icon({
        iconUrl: 'https://image.flaticon.com/icons/png/512/1493/1493766.png',
        iconSize:     [55, 45],
        shadowSize:   [50, 64],
        iconAnchor:   [28, 22],
        shadowAnchor: [4, 62],
        popupAnchor:  [-3, -76]
      }),
      pompes: null,
      bicloos: null,
      abris: null,
      filtres: [{
        ctg: "pompes",
        display: true,
      }, {
        ctg: "bicloos",
        display: true,
      }, {
        ctg: "abris",
        display: true,
      }]
    };
  },
  created() {
    //do we support geolocation
    if(!("geolocation" in navigator)) {
      this.errorStr = 'Geolocation is not available.';
      return;
    }

    this.gettingLocation = true;
    // get position
    navigator.geolocation.getCurrentPosition(pos => {
      this.gettingLocation = false;
      this.location = [pos.coords.latitude, pos.coords.longitude];
    }, err => {
      this.gettingLocation = false;
      this.errorStr = err.message;
    })
  },
  mounted() {
    // const urlProxy = "https://cors-anywhere.herokuapp.com/";

    const getActualPompes = () => {
      const dataPompes = "https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_gonfleurs-libre-service-nantes-metropole&rows=32";
      axios
        .get(dataPompes)
        .then(res => {
          this.pompes = res.data.records;
        })
    }

    const getActualBikes = () => {
      const dataBicloo = "https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_stations-velos-libre-service-nantes-metropole-disponibilites&rows=123";
      axios
        .get(dataBicloo)
        .then(res => {
          this.bicloos = res.data.records;
        })
    }

    const getActualAbris = () => {
      const dataAbris = "https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_abris-velos-nantes-metropole&rows=193";
      axios
        .get(dataAbris)
        .then(res => {
          this.abris = res.data.records;
        })
    }

    getActualPompes();
    getActualBikes();
    getActualAbris();
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
    },
    changeFiltre(obj) {
      let filtre = 0;
      for (let i = 0; i < this.filtres.length; i++) {
        if (this.filtres[i].ctg == obj) {
          filtre = i;
        };
      }
      this.filtres[filtre].display = !this.filtres[filtre].display;
    },
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
  nav {
    display: flex;
    justify-content: space-around;
    align-items: center;
    background-color: #FFB482;
    height: 6%;
    width: 100vw;
  }

  h1 {
    font-weight: 600;
    color: #4D5DE8;
  }

  button {
    height: 40px;
  }
</style>