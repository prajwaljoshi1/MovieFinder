<template>
  <div class="moviedetails">
    <h1 class="moviedetails__title">{{movieDetails.Title}}</h1>
    <div class="moviedetails__title">{{movieDetails.Genre}}</div>
    <p class="moviedetails__plot">Movie plot - {{movieDetails.Plot}}</p>
    <div class="moviedetails__language"><span>Language: </span>{{movieDetails.Language}}</div>
    <div class="moviedetails__actors"><span>Actors: </span>{{movieDetails.Actors}}</div>
    <div class="moviedetails__duration"><span>Duration: </span>{{movieDetails.Runtime}}</div>
    <div class="moviedetails__poster">
      <img :src="movieDetails.Poster" :alt="movieDetails.Title">
    </div>
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
        movieDetails: {},
    };
  },
  watch: {
    async selectedMovieId(id){
      console.log('id ', id);  // eslint-disable-line
      let response = await fetch(`http://www.omdbapi.com/?apikey=971470b1&i=${id}&type=movie&plot=full`);
      let data = await response.json();
      if (data.Response === 'True') {
        this.movieDetails = data;
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
