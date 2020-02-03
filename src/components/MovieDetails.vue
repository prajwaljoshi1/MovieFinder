<template>
  <div v-if="selectedMovieId.length > 1" class="moviedetails">
    <button class="btn moviedetails__back" v-on:click="closeDetails"><close class="svg"/></button>
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
import Close from './icons/Close.vue';

export default {
  name: 'Moviedetails',
  components: { Close },
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
      let response = await fetch(`http://www.omdbapi.com/?apikey=${process.env.VUE_APP_OMDB_API_KEY}&i=${id}&type=movie&plot=full`);
      let data = await response.json();
      if (data.Response === 'True') {
        this.movieDetails = data;
      } else {
        console.log("ERROR!", JSON.stringify(data));  // eslint-disable-line
      }
    }
  },
  methods: {
    closeDetails() {
       this.$emit("closeDetails", true);
    }
  }
}
</script>

<style lang="scss">

  .moviedetails {
    display: flex;
    justify-content: space-between;
    position: relative;
    @media only screen and (max-width: 900px) {
      flex-direction: column-reverse;
      justify-content: flex-start;

    }

    &__text{
      flex: 1 1 auto;
      text-align: left;
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
      @media only screen and (max-width: 900px) {
       flex: 0;
      }
    }

    &__image{
      border: 2px solid #111;
      max-width:30rem;
      max-height: 40rem;
      display: block;
      margin: 0 auto;
            
      @media only screen and (max-width: 1100px) {
        max-width:25rem;
        max-height: 32rem;
      }
    }

    &__noimage {
      text-align: center;
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

    &__back {
      position: absolute;
      top: 2rem;
      right: 2rem;
      border-radius: 50%;
      background-color: #222;
      padding: 1rem;
      @media only screen and (min-width: 750px) {
        display: none;
      }


      & .svg {
        fill: #eee;
      }
    }
  
  }
</style>
