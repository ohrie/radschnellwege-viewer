<template>
  <header>
    <img
      alt="Radschnellwege Logo"
      class="brand-logo"
      src="assets/radschnellwege-logo.svg"
    />
    <span class="brand-name">Radschnellwege Deutschland</span>
    <RsvDetails />
  </header>
  <Map />
</template>

<script>
import Map from "./components/Map.vue";
import RsvDetails from "./components/RsvDetails.vue";

export default {
  name: "Radschnellwege",
  components: {
    Map,
    RsvDetails
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
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.5rem;
  background: white;
  height: 20vh;
  box-sizing: border-box;

  @media screen and (min-width: 750px) {
    position: absolute;
    margin: 0.5rem;
    border-radius: 0.5rem;
    z-index: 10;
  }
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
</style>
