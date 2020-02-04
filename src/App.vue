<template>
  <div class="app">
    <search-panel
      v-show="showSearchPanel"
      class="app__search-panel"
      v-on:movieSelected="setSelectedMovie"
      v-on:displaySelectedMovieDetails="displaySelectedMovieDetails"
    />
    <movie-details
      v-show="showMovieDetailsPanel"
      class="app__details-panel"
      :selectedMovieId="selectedMovieId"
      v-on:closeDetails="hideMovieDetails"
      :showMovieDetailsData="showMovieDetailsData"
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
      selectedMovieId: "",           // imdb id of selected movie
      showSearchPanel: true,         // show searchpanel, hide in mobile devices when details page is shown
      showMovieDetailsPanel: false,  // show movie details panel, hide when no movie is selected on initial app load
      showMovieDetailsData: false    //  sho data in moie details, hide when new movie has been searched but no movie is selected from the new list
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
      this.showMovieDetailsPanel = id.length > 0;
      this.showSearchPanel = window.innerWidth >= 750;
      this.showMovieDetailsData = true;
    },
    hideMovieDetails() {
      this.showSearchPanel = true;
      this.showMovieDetailsPanel = false;
    },
    searchPanalShowHide(event) {
      this.showSearchPanel = !(
        event.target.innerWidth < 750 && this.showMovieDetailsPanel
      );
    },
    displaySelectedMovieDetails(value){
      this.showMovieDetailsData = value;
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
  background: linear-gradient(to top, #00c6ff, #0072ff);
}

.app {
  display: flex;
  margin: 2.5rem;
  border: 1px solid #fff;
  border-radius: 8px;
  background-color: rgba(255, 255, 255, .8); 
  color: #444;

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
    border-left: 2px solid #0072ff;
    padding: 4rem 4rem 1rem 4rem;
    min-height: 95vh;
  }
}
</style>
