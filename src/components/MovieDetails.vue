<template>
  <div v-if="selectedMovieId.length > 1" class="moviedetails">
    <div class="moviedetails__text">
      <h1 class="moviedetails__title">{{movieDetails.Title}}</h1>
      <div class="moviedetails__genre">{{movieDetails.Genre}}</div>
      <p class="moviedetails__plot">Movie plot - {{movieDetails.Plot}}</p>
      <div class="moviedetails__furtherinfo"><span>Language: </span>{{movieDetails.Language}}</div>
      <div class="moviedetails__furtherinfo"><span>Actors: </span>{{movieDetails.Actors}}</div>
      <div class="moviedetails__furtherinfo"><span>Duration: </span>{{movieDetails.Runtime}}</div>
    </div>
    <div class="moviedetails__poster">
      <img  v-if="movieDetails.Poster !== 'N/A'"  class="moviedetails__image" :src="movieDetails.Poster" :alt="movieDetails.Title">
      <div v-else class="moviedetails__noimage">Image not available.</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Moviedetails',
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
    display: flex;
    justify-content: space-between;
    @media only screen and (max-width: 900px) {
      flex-direction: column;
    }

    &__text{
      flex: 1;
      width: 60%;
      text-align: left;
      padding: 0 1rem 0 0;
    }

    &__title{
      font-size: 3.5rem;
      font-weight: 800;
      @media only screen and (max-width: 1100px) {
        font-size: 2.5rem;
      }
    }

    &__genre {
      font-size: 1.4rem;
      font-weight: 500;
      margin: 2rem 0;
      @media only screen and (max-width: 1100px) {
        font-size: 1.2rem;
      }
    }

    &__plot {
      font-size: 1.6rem;
      font-weight: 500;
      margin: 2rem 0;
      @media only screen and (max-width: 1100px) {
        font-size: 1.3rem;
      }
    }

    &__furtherinfo{
      margin: 0.5rem 0;
      font-size: 1.6rem;
      & span {
        font-weight: 800;
      }
      @media only screen and (max-width: 1100px) {
        font-size: 1.3rem;
      }
    }

    &__poster{
      padding: 1rem;
      flex: 1 0 40;
      display: flex;
      // flex-basis: auto;
      @media only screen and (max-width: 1200px) {
       flex: 0 0 50%;
      }
    }

    &__image{
      border: 2px solid #111;
      max-width:30rem;
      max-height: 40rem;
            
      @media only screen and (max-width: 1100px) {
        max-width:25rem;
        max-height: 32rem;
      }
    }

    &__noimage {
      border: 2px solid #111;
      height: 40rem;
      width: 30rem;
      padding-top: 16rem;
      font-size: 2rem;
      @media only screen and (max-width: 1100px) {
        max-height:32rem;
        max-width: 25rem;
      }
    }

  }
</style>
