<template>
  <div class="rsvDetails">
    <div class="rsvMainDetails">
      <span class="rsv-shield" v-if="rsvData.ref">{{ rsvData.ref }}</span>
      <span class="rsv-shield" v-else>?</span>
      <span class="rsvName" v-if="rsvData.name">{{ rsvData.name }}</span>
      <span class="rsvName" v-else>{{ rsvData.from }} ↔ {{ rsvData.to }}</span>
    </div>
    <div class="rsvSecondaryDetails">
      <ul>
        <li v-if="rsvData.name && rsvData.from">{{ rsvData.from }} ↔ {{ rsvData.to }}</li>
        <li v-if="rsvData.length">{{ rsvData.length }}km</li>
        <!--<li v-if="rsvData.state">Status: {{ rsvData.state }}</li>-->
        <li v-if="rsvData.state != built"><i>Trassenverlauf nicht final</i></li>
        <li v-if="rsvData.copyright === 'OpenSreetMap'">© OpenStreetMap-Mitwirkende</li>
      </ul>
    </div>
    <div class="flex bottom-links">
      <span v-if="rsvData.accuracy" class="dimmed-text">Genauigkeit: {{ rsvData.accuracy }}</span>
      <span class="default-link" v-if="rsvData.website"><a :href="rsvData.website" target="_blank" rel="noopener noreferrer">
        <i class="fas fa-globe-europe"></i> Mehr Infos</a>
      </span>
      <span class="default-link" v-if="rsvData.pilot_study" title="Link zur Machbarkeitsstudie">
        <a :href="rsvData.pilot_study" target="_blank" rel="noopener noreferrer">
          <i class="fas fa-file-pdf"></i> PDF
        </a>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: "RsvDetails",
  props: ["rsvData"],
  mounted() {},
  methods: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$rsv-green: #008754;

.rsv-shield {
  background-color: $rsv-green;
  border-radius: 6px;
  border: white 3px solid;
  outline: $rsv-green 3px solid;
  color: white;
  font-weight: bold;
  font-size: 2rem;
  padding: 4px 4px 0 4px;
  margin-right: 1rem;
}

.rsvMainDetails {
  display: flex;
  justify-content: left;
  align-items: center;

  .rsvName {
    font-weight: bold;
  }
}

.rsvSecondaryDetails ul {
  list-style: none;
}

.default-link {
  color: grey;

  &:hover,
  &:active,
  &:visited {
    color: grey;
  }
}

.bottom-links {
  justify-content: space-between;
}
</style>
