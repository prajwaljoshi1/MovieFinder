<template>
  <div id="app">
    <div class="container">
      <div class="search-panel">
        <movie-input v-on:movieTitleChange="searchMovie" />
        <movie-list v-if="!hasSearchError " class="search-panel__movie-list" v-on:movieSelected="setSelectedMovie" :searchResult="searchResult" />
        <div v-else class=" search-panel__error">{{searchErrorMsg}}</div>
        <pagination v-if="showPagination" :totalResults="totalResults" :pageNumber="pageNumber" v-on:pageChange="pageChange"/>
      </div>
      <div class="details-panel">
        <movie-details :selectedMovieId="selectedMovieId" />
      </div>
    </div>
  </div>
</template>

<script>
import MovieInput from './components/MovieInput.vue';
import MovieList from './components/MovieList.vue';
import MovieDetails from './components/MovieDetails.vue';
import Pagination from './components/Pagination.vue';

export default {
  name: 'app',
  components: { MovieInput, MovieList, MovieDetails, Pagination },
  data() {
    return {
      selectedMovieId: '',
      lastSearchedMovieName: '',
      hasSearchError: false,
      searchErrorMsg: '',
      searchResult: [],
      totalResults: 0,
      pageNumber: 1,
      showPagination: false
    }
  },
  methods: {
    async searchMovie(movieName){
      this.hasSearchError = false;
      this.searchErrorMsg = '';
      let response = await fetch(`http://www.omdbapi.com/?apikey=971470b1&s=${movieName}&type=movie&page=${this.pageNumber}`);
      let data = await response.json();
      if (data.Response === 'True') {
        this.lastSearchedMovieName = movieName;
        this.searchResult = data.Search;
        this.totalResults = data.totalResults;
        this.showPagination = data.totalResults > 10 

        console.log(data, this.searchResult);  // eslint-disable-line
      } else {
        console.log("ERROR!", JSON.stringify(data));  // eslint-disable-line
        this.hasSearchError = true;
        this.searchErrorMsg = data.Error;
        this.showPagination = false;
      }
    },
    pageChange(pageNumber){
      this.pageNumber = pageNumber;
      this.searchMovie(this.lastSearchedMovieName);
    },
    setSelectedMovie(id) {
      this.selectedMovieId = id;
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

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}

.container {
  display: flex;
  margin: 2.5rem;
  border: 2px solid #111;
  border-radius: 4px;
}

.search-panel {
  flex: 0 0 25%;
  border-right: 2px solid #111;;
  padding: 4rem 2.5rem 1rem 2.5rem;
  min-height: 90vh;

  display: flex;
  flex-direction: column;
  justify-content: flex-start;

  &__movie-list {
    margin-bottom: auto;
    flex-grow: 1; 
  }

  &__error {
    margin-top: 5rem;
    font-size: 2rem;
    color: red;
    margin-bottom: auto;
    flex-grow: 1; 
  }
}

.details-panel {
  flex: 1;
}

</style>
