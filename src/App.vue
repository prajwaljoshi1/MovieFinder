<template>
  <div id="app">
    <div class="container">
      <div class="search-panel">
        <MovieInput/>
        <MovieList/>
      </div>
      <div class="details-panel">
        <MovieDetails/>
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
  components: { MovieInput, MovieList, MovieDetails }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}

.container {
  display: flex;
  margin: 60px;
}

.search-panel {
  background-color: orangered;
  flex: 0 0 30%;
}

.details-panel {
  background-color: limegreen;
  flex: 1;
}










</style>
