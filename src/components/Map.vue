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
    createMap() {
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

      // Add hover/click style
      /*this.map.addSource("radschnellwege-data", {
        type: "vector",
        url: "mapbox://henri97.ckrtk8ljd0p2a28pk6xh9zsfo-1zxi9",
      });
      this.map.addLayer({
        id: "radschnellwege-highlighted",
        type: "line",
        source: "radschnellwege-data",
        "source-layer": "Radschnellwege",
        paint: {
          "line-width": 10,
          "line-color": "#ffffff",
          "line-opacity": [
            "case",
            ["boolean", ["feature-state", "hover"], false],
            1,
            0,
          ],
        },
        layout: {
          "line-join": "round",
          "line-cap": "round",
        },
      });

      let hoveredElementId;
      this.map.on("mousemove", "radschnellwege-highlighted", (e) => {
        if (e.features.length > 0) {
          if (hoveredElementId) {
            this.map.setFeatureState(
              {
                source: "radschnellwege-data",
                sourceLayer: "Radschnellwege",
                id: hoveredElementId,
              },
              { hover: false }
            );
          }
          hoveredElementId = e.features[0].id;
          this.map.setFeatureState(
            {
              source: "radschnellwege-data",
              sourceLayer: "Radschnellwege",
              id: hoveredElementId,
            },
            { hover: true }
          );
        }
      });

      this.map.on("mouseleave", "radschnellwege-highlighted", () => {
        if (hoveredElementId) {
          this.map.setFeatureState(
            {
              source: "radschnellwege-data",
              sourceLayer: "Radschnellwege",
              id: hoveredElementId,
            },
            { hover: false }
          );
        }
        hoveredElementId = null;
      });*/
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
