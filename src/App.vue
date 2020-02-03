<template>
  <div class="app">
    <search-panel
      v-show="showSearchPanel"
      class="app__search-panel"
      v-on:movieSelected="setSelectedMovie"
    />
    <movie-details
      v-show="showMovieDetails"
      class="app__details-panel"
      :selectedMovieId="selectedMovieId"
      v-on:closeDetails="hideMovieDetails"
    />
  </div>
</template>

<script>
import SearchPanel from "./components/SearchPanel.vue";
import MovieDetails from "./components/MovieDetails.vue";

export default {
  name: "app",
  components: { SearchPanel, MovieDetails },
  data() {
    return {
      selectedMovieId: "",
      showSearchPanel: true,
      showMovieDetails: false
    };
  },
  created() {
    window.addEventListener("resize", this.searchPanalShowHide);
  },
  destroyed() {
    window.removeEventListener("resize", this.searchPanalShowHide);
  },
  methods: {
    setSelectedMovie(id) {
      this.selectedMovieId = id;
      this.showMovieDetails = id.length > 0;
      this.showSearchPanel = window.innerWidth >= 750;
    },
    hideMovieDetails() {
      this.showSearchPanel = true;
      this.showMovieDetails = false;
    },
    searchPanalShowHide(event) {
      this.showSearchPanel = !(
        event.target.innerWidth < 750 && this.showMovieDetails
      );
    }
  }
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
  font-size: 62.5%; //1rem = 10px
}

body {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
}

.app {
  display: flex;
  margin: 2.5rem;
  border: 2px solid #111;

  &__search-panel {
    flex: 1;
    padding: 4rem 2.5rem 1rem 2.5rem;
    min-height: 95vh;
    min-width: 35rem;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  &__details-panel {
    flex: 1 1 75%;
    border-left: 2px solid #111;
    padding: 4rem 4rem 1rem 4rem;
    min-height: 95vh;
  }
}
</style>
