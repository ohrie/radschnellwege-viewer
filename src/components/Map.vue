<template>
  <div>
    <div id="map"></div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

export default {
  name: "Map",
  props: {},
  data() {
    return {
      access_token: process.env.VUE_APP_MAPBOX_ACCESS_TOKEN,
      center: [10.552, 50.730],
    };
  },
  mounted() {
    this.createMap();
    this.initHoverEffect();
  },
  methods: {
    async createMap() {
      mapboxgl.accessToken = this.access_token;
      this.map = new mapboxgl.Map({
        container: "map",
        style: process.env.VUE_APP_MAPBOX_STLYE,
        zoom: 5.5,
        center: this.center,
      });
    },
    initHoverEffect() {
      this.map.on("mousemove", "radschnellwege", (e) => {
        if (e.features.length > 0) {
          this.map.getCanvas().style.cursor = 'pointer';
        }
      });

      // When the mouse leaves the state-fill layer, update the feature state of the
      // previously hovered feature.
      this.map.on("mouseleave", "radschnellwege", () => {
        this.map.getCanvas().style.cursor = '';
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#map {
  height: 80vh;
  width: 100%;
  margin: auto;

  @media screen and (min-width: 750px) {
    height: 100vh;
  }
}
</style>
