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
    this.initHoverEffect();
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
      this.map.addControl(new mapboxgl.NavigationControl(), 'bottom-right');
    },
    initHoverEffect() {
      this.map.on("mousemove", "radschnellwege", (e) => {
        if (e.features.length > 0) {
          this.map.getCanvas().style.cursor = "pointer";
        }
      });

      // When the mouse leaves the state-fill layer, update the feature state of the
      // previously hovered feature.
      this.map.on("mouseleave", "radschnellwege", () => {
        this.map.getCanvas().style.cursor = "";
      });

      this.map.on("load", () => {
        /*this.map.addLayer({
          id: "radschnellwege-highlighted",
          type: "line",
          source: "radschnellwege",
          "source-layer": "radschnellwege",
          paint: {
            "line-width": 8,
            "line-color": "#6e599f",
            "line-opacity": 1.0,
          },
          filter: ["in", "ref", ""],
        });*/
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
          this.$emit('rsvclicked', props[0]);
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

  @media screen and (min-width: 750px) {
    height: 100vh;
  }
}
</style>
