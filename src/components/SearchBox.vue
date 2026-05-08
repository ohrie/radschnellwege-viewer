<template>
  <div class="search-box">
    <input
      ref="input"
      type="text"
      v-model="query"
      @input="onInput"
      @keydown.down.prevent="moveDown"
      @keydown.up.prevent="moveUp"
      @keydown.enter.prevent="confirmSelection"
      @keydown.escape="closeSuggestions"
      @blur="closeSuggestionsDelayed"
      placeholder="Ort suchen..."
      autocomplete="off"
      spellcheck="false"
      class="search-input"
    />
    <ul v-if="suggestions.length > 0" class="suggestions-list" role="listbox">
      <li
        v-for="(suggestion, index) in suggestions"
        :key="index"
        :class="{ active: index === activeIndex }"
        role="option"
        @mousedown.prevent="selectSuggestion(suggestion)"
        @mouseover="activeIndex = index"
      >
        <span class="suggestion-name">{{ suggestion.properties.name }}</span>
        <span class="suggestion-detail">{{ formatDetail(suggestion) }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SearchBox",
  emits: ["location-selected"],
  data() {
    return {
      query: "",
      suggestions: [],
      activeIndex: -1,
      userLocation: null,
      debounceTimer: null,
    };
  },
  mounted() {
    this.requestUserLocation();
  },
  methods: {
    requestUserLocation() {
      if (!navigator.geolocation) return;
      navigator.geolocation.getCurrentPosition(
        (pos) => {
          this.userLocation = {
            lat: pos.coords.latitude,
            lon: pos.coords.longitude,
          };
        },
        () => {}
      );
    },
    onInput() {
      clearTimeout(this.debounceTimer);
      this.activeIndex = -1;
      if (this.query.trim().length < 2) {
        this.suggestions = [];
        return;
      }
      this.debounceTimer = setTimeout(() => this.fetchSuggestions(), 300);
    },
    async fetchSuggestions() {
      const params = new URLSearchParams({
        q: this.query,
        limit: 6,
        lang: "de",
      });
      if (this.userLocation) {
        params.set("lat", this.userLocation.lat);
        params.set("lon", this.userLocation.lon);
      }
      try {
        const res = await fetch(
          `https://photon.komoot.io/api/?${params}`
        );
        const data = await res.json();
        this.suggestions = data.features || [];
      } catch {
        this.suggestions = [];
      }
    },
    formatDetail(feature) {
      const p = feature.properties;
      const parts = [p.city || p.town || p.village, p.state, p.country].filter(Boolean);
      return parts.join(", ");
    },
    moveDown() {
      if (this.suggestions.length === 0) return;
      this.activeIndex = (this.activeIndex + 1) % this.suggestions.length;
    },
    moveUp() {
      if (this.suggestions.length === 0) return;
      this.activeIndex =
        this.activeIndex <= 0
          ? this.suggestions.length - 1
          : this.activeIndex - 1;
    },
    confirmSelection() {
      if (this.activeIndex >= 0 && this.activeIndex < this.suggestions.length) {
        this.selectSuggestion(this.suggestions[this.activeIndex]);
      }
    },
    selectSuggestion(feature) {
      const [lon, lat] = feature.geometry.coordinates;
      this.query = feature.properties.name || "";
      this.suggestions = [];
      this.activeIndex = -1;
      this.$emit("location-selected", { lat, lon });
    },
    closeSuggestions() {
      this.suggestions = [];
      this.activeIndex = -1;
    },
    closeSuggestionsDelayed() {
      setTimeout(() => this.closeSuggestions(), 150);
    },
  },
};
</script>

<style scoped lang="scss">
.search-box {
  position: relative;
  margin-top: 0.75rem;
}

.search-input {
  width: 100%;
  box-sizing: border-box;
  padding: 0.5rem 0.75rem;
  border: 1px solid #ddd;
  border-radius: 0.375rem;
  font-size: 0.95rem;
  font-family: inherit;
  outline: none;
  transition: border-color 0.15s;

  &:focus {
    border-color: #4a90d9;
    box-shadow: 0 0 0 2px rgba(74, 144, 217, 0.2);
  }
}

.suggestions-list {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border: 1px solid #ddd;
  border-top: none;
  border-radius: 0 0 0.375rem 0.375rem;
  list-style: none;
  margin: 0;
  padding: 0;
  z-index: 100;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);
  max-height: 260px;
  overflow-y: auto;

  li {
    padding: 0.5rem 0.75rem;
    cursor: pointer;
    display: flex;
    flex-direction: column;
    gap: 0.1rem;
    border-bottom: 1px solid #f0f0f0;

    &:last-child {
      border-bottom: none;
    }

    &:hover,
    &.active {
      background: #f0f6ff;
    }
  }
}

.suggestion-name {
  font-size: 0.9rem;
  font-weight: 500;
  color: #2c3e50;
}

.suggestion-detail {
  font-size: 0.75rem;
  color: #888;
}
</style>
