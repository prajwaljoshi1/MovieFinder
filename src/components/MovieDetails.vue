<template>
  <div>
    <button class="btn moviedetails__back" v-on:click="closeDetails"><close class="svg" /></button>
    <div v-if="selectedMovieId.length > 1 && !hasError && showMovieDetailsData" class="moviedetails">
      <div class="moviedetails__text">
        <h1 class="moviedetails__title">{{movieDetails.Title}}</h1>
        <div class="moviedetails__genre">{{movieDetails.Genre}}</div>
        <p class="moviedetails__plot">Movie plot - {{movieDetails.Plot}}</p>
        <div class="moviedetails__furtherinfo"><span>Language: </span>{{movieDetails.Language}}</div>
        <div class="moviedetails__furtherinfo"><span>Actors: </span>{{movieDetails.Actors}}</div>
        <div class="moviedetails__furtherinfo"><span>Duration: </span>{{movieDetails.Runtime}}</div>
      </div>
      <div class="moviedetails__poster">
        <img
          v-if="movieDetails.Poster !== 'N/A'"
          class="moviedetails__image"
          :src="movieDetails.Poster"
          :alt="movieDetails.Title"
        />
        <div v-else class="moviedetails__noimage">Image not available.</div>
      </div>
    </div>
    <div v-if="hasError" class="moviedetails__error">{{errorMessage}}</div>
      <div v-if="!showMovieDetailsData" class="moviedetails__message">Select a movie from the list to see details.</div>
  </div>
</template>

<script>
import Close from "./icons/Close.vue";

export default {
  name: "Moviedetails",
  components: { Close },
  props: {
    selectedMovieId: String,
    showMovieDetailsData: Boolean,
  },
  data() {
    return {
      movieDetails: {},   // object containng result of fetch get movie info         
      hasError: false,    // fetch api for getting movie info has error
      errorMessage: ''    // fetch api for getting movie info error message
    };
  },
  watch: {
    async selectedMovieId(id) {
      this.hasError = false;
      this.errorMessage = '';
      let response = await fetch( `http://www.omdbapi.com/?apikey=${process.env.VUE_APP_OMDB_API_KEY}&i=${id}&type=movie&plot=full`);
      let data = await response.json();
      if (data.Response === "True") {
        this.movieDetails = data;
      } else {
        this.hasError = true;
        this.errorMessage = data.Error || 'Something went wrong.';
      }
    }
  },
  methods: {
    closeDetails() {
      this.$emit("closeDetails", true);
    }
  }
};
</script>

<style lang="scss">
.moviedetails {
  display: flex;
  justify-content: space-between;
  @media only screen and (max-width: 900px) {
    flex-direction: column-reverse;
    justify-content: flex-start;
  }

  &__text {
    flex: 1 1 auto;
    text-align: left;
  }

  &__title {
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
    color: #0072ff;
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

  &__furtherinfo {
    margin: 0.5rem 0;
    font-size: 1.6rem;
    & span {
      font-weight: 800;
    }
    @media only screen and (max-width: 1100px) {
      font-size: 1.3rem;
    }
  }

  &__poster {
    padding: 1rem;
    flex: 1 0 40;
    display: flex;
    // flex-basis: auto;
    @media only screen and (max-width: 900px) {
      flex: 0;
    }
  }

  &__image {
    border: 2px solid #666;
    max-width: 30rem;
    max-height: 40rem;
    display: block;
    margin: 0 auto;

    @media only screen and (max-width: 1100px) {
      max-width: 25rem;
      max-height: 32rem;
    }
  }

  &__noimage {
    text-align: center;
    border: 2px solid #666;
    height: 40rem;
    width: 30rem;
    padding-top: 16rem;
    font-size: 2rem;
    display: block;
    margin: 0 auto;
    @media only screen and (max-width: 1100px) {
      max-height: 32rem;
      max-width: 25rem;
    }
  }

  &__back {
    position: absolute;
    top: 4rem;
    right: 4rem;
    border-radius: 0.8rem;
    background-color: #222;
    padding: 1rem;
    @media only screen and (min-width: 750px) {
      display: none;
    }

    & .svg {
      fill: #eee;
    }
  }

  &__error{
    text-align: center;
    margin-top: 5rem;
    font-size: 2rem;
    color: red;
    min-width: 10rem;
  }

    &__message{
    text-align: center;
    margin-top: 5rem;
    font-size: 2rem;
    color: #444;
    min-width: 10rem;
  }
}
</style>
