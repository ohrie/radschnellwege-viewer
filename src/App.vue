<template>
  <header>
    <button @click="hideRsvDetails()" v-if="!showPageHeadline" class="rsv_close_button">X</button>
    <div class="brand" v-if="showPageHeadline">
      <img
        alt="Radschnellwege Logo"
        class="brand-logo"
        src="assets/radschnellwege-logo.svg"
      />
      <div class="brand-name">Radschnellwege Deutschland</div>
    </div>
    <RsvDetails :rsvData="rsvData" v-if="!showPageHeadline"/>
  </header>
  <Map @rsvclicked="showData" />
</template>

<script>
import Map from "./components/Map.vue";
import RsvDetails from "./components/RsvDetails.vue";
import "@fortawesome/fontawesome-free/css/all.css"

export default {
  name: "Radschnellwege",
  data() {
    return {
      rsvData: {},
      showPageHeadline: true
    };
  },
  components: {
    Map,
    RsvDetails,
  },
  methods: {
    showData(event) {
      this.rsvData = event;
      this.showPageHeadline = false;
    },
    hideRsvDetails() {
      this.showPageHeadline = true;
    }
  },
};
</script>

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

header {
  background: white;
  padding: 1rem;
  width: 400px;

  //@media screen and (min-width: 750px) {
    position: absolute;
    margin: 0.5rem;
    border-radius: 0.5rem;
    z-index: 10;
    height: auto;
  //}

  @media screen and (max-width: 750px) {
    width: calc(100% - 3em);
  }
}

.brand {
  display: flex;
  align-items: center;
  justify-content: center;

  box-sizing: border-box;
}

.brand-logo {
  height: 50px;
  margin-right: 0.5rem;
}

.brand-name {
  font-size: 1.5rem;
  font-weight: bold;
  margin-left: 0.5rem;
}

.rsv_close_button {
  position: absolute;
  right: 0;
  top: 0;

  @media screen and (max-width: 750px) {
    padding: 10px;
  }
}

.dimmed-text {
  color: rgb(170, 170, 170);
}

.flex {
  display: flex;
}
</style>
