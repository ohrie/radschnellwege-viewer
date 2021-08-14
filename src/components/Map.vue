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
      center: [10.552, 50.73],
    };
  },
  mounted() {
    this.createMap();
    this.map.on("load", () => {
      this.initHoverEffect();
    });
  },
  methods: {
    async createMap() {
      mapboxgl.accessToken = this.access_token;
      this.map = new mapboxgl.Map({
        container: "map",
        style: process.env.VUE_APP_MAPBOX_STYLE,
        zoom: 5.5,
        center: this.center,
      });
      this.map.addControl(new mapboxgl.NavigationControl(), "bottom-right");
    },
    initHoverEffect() {
      this.map.on("mousemove", "radschnellwege", (e) => {
        if (e.features.length > 0) {
          this.map.getCanvas().style.cursor = "pointer";
        }
      });

      this.map.on("mouseleave", "radschnellwege", () => {
        this.map.getCanvas().style.cursor = "";
      });

      this.map.on("click", (e) => {
        // Set `bbox` as 5px reactangle area around clicked point.
        const bbox = [
          [e.point.x - 5, e.point.y - 5],
          [e.point.x + 5, e.point.y + 5],
        ];
        // Find features intersecting the bounding box.
        const selectedFeatures = this.map.queryRenderedFeatures(bbox, {
          layers: ["radschnellwege"],
        });
        const props = selectedFeatures.map((feature) => feature.properties);
        if (props.length > 0) {
          this.$emit("rsvclicked", props[0]);
        }
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#map {
  height: 100vh;
  width: 100%;
  margin: auto;
}
</style>
