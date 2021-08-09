<template>
  <div>
    {{ rsvData.name }}
    {{ rsvData.accuracy }}
    {{ rsvData.ref }}
  </div>
</template>

<script>

export default {
  name: "RsvDetails",
  props: ["rsvData"],
  mounted() {},
  methods: {
    showData(event) {
      console.warn("HADSF", event);
    }
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
