<template>
  <div class="hello">
    <h1>{{selectedMovieId}}</h1>
  </div>
</template>

<script>
export default {
  name: 'MovieDetails',
  props: {
    selectedMovieId: String,
  },
  data() {
    return {

    };
  },
  methods: {
    async searchMovie(movieName){
      let response = await fetch(`http://www.omdbapi.com/?i=tt3896198&apikey=971470b1&s=${movieName}&type=movie&page=${this.pageNumber}`);
      let data = await response.json();
      if (data.Response === 'True') {
        this.searchResult = data.Search;
        this.totalResults = data.totalResults;
        console.log(data, this.searchResult);  // eslint-disable-line
      } else {
        console.log("ERROR!", JSON.stringify(data));  // eslint-disable-line
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
