<template>
  <div id="app">
    <div class="container">
      <div class="search-panel">
        <movie-input v-on:movieTitleChange="searchMovie" />
        <movie-list/>
      </div>
      <div class="details-panel">
        <movie-details/>
      </div>
    </div>
  </div>
</template>

<script>
import MovieInput from './components/MovieInput.vue';
import MovieList from './components/MovieList.vue';
import MovieDetails from './components/MovieDetails.vue';

export default {
  name: 'app',
  components: { MovieInput, MovieList, MovieDetails },
  data() {
    return {
      selectedMovie: null,
      searchResult: [],
      totalResults: 0,
      pageNumber: 1
    }
  },
  methods: {
    async searchMovie(movieName){
      let response = await fetch(`http://www.omdbapi.com/?i=tt3896198&apikey=971470b1&s=${movieName}&type=movie&page=${this.pageNumber}`);
      let data = await response.json();
      if (data.Response === 'True') {
        this.searchResult = data.search;
        this.totalResults = data.totalResults;
        console.log(data);  // eslint-disable-line
      } else {
        console.log("ERROR!");  // eslint-disable-line
      }
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
  margin: 5rem 2.5rem;
  border: 2px solid grey;
  border-radius: 4px;
}

.search-panel {
  flex: 0 0 30%;
  border-right: 2px solid grey;;
  padding: 5rem 2.5rem;
}

.details-panel {
  flex: 1;
}










</style>
