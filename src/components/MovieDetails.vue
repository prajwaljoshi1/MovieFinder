<template>
  <div class="moviedetails">
    <div class="moviedetails__text">
      <h1 class="moviedetails__title">{{movieDetails.Title}}</h1>
      <div class="moviedetails__genre">{{movieDetails.Genre}}</div>
      <p class="moviedetails__plot">Movie plot - {{movieDetails.Plot}}</p>
      <div class="moviedetails__furtherinfo"><span>Language: </span>{{movieDetails.Language}}</div>
      <div class="moviedetails__furtherinfo"><span>Actors: </span>{{movieDetails.Actors}}</div>
      <div class="moviedetails__furtherinfo"><span>Duration: </span>{{movieDetails.Runtime}}</div>
    </div>
    <div class="moviedetails__poster">
      <img class="moviedetails__image" :src="movieDetails.Poster" :alt="movieDetails.Title">
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

<style lang="scss">
  .moviedetails {
    margin: 5rem;
    display: flex;
    justify-content: space-between;

    &__text{
      flex: 0 0 60%;
      text-align: left;
    }

    &__title{
      font-size: 4rem;
      font-weight: 800;
    }

    &__genre {
      font-size: 1.4rem;
      font-weight: 500;
      margin: 2rem 0;
    }

    &__plot {
      font-size: 1.6rem;
      font-weight: 500;
      margin: 2rem 0;
    }

    &__furtherinfo{
      margin: 0.5rem 0;
      font-size: 1.6rem;
      & span {
        font-weight: 800;
      }
    }

    &__poster{
      flex: 0;
      display: flex;
    }

    &__image{
      border: 2px solid #111;
    }

  }
</style>
