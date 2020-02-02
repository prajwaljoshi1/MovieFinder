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
        v-on:back="back"
      />

    </div>
</template>

<script>
import SearchPanel from './components/SearchPanel.vue';
import MovieDetails from './components/MovieDetails.vue';

export default {
  name: 'app',
  components: { SearchPanel, MovieDetails },
  data() {
    return {
      selectedMovieId: '',
      showSearchPanel: true,
      showMovieDetails: false
    }
  },
  methods: {
    setSelectedMovie(id) {
      this.selectedMovieId = id;
      this.showMovieDetails = id.length > 0;
    },
    back() {
      this.showSearchPanel = true;
    }
  }
}
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
   min-width:400px;        /* Suppose you want minimum width of 1000px */
   width: auto !important;  /* Firefox will set width as auto */
   width:400px; 
   font-family: 'Avenir', Helvetica, Arial, sans-serif;
}


.app {
  display: flex;
  margin: 2.5rem;
  border: 2px solid #111;
  border-radius: 4px;

  &__search-panel {
    flex: 1;
    padding: 4rem 2.5rem 1rem 2.5rem;
    min-height: 95vh;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  &__details-panel {
    flex: 0 1 75%;
    // flex-basis: 70rem;
    border-left: 2px solid #111;
    padding: 4rem 4rem 1rem 4rem;
  }
}

</style>
