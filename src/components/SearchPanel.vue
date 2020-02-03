<template>
  <div class="searchpanel">
    <movie-input v-on:movieTitleChange="searchMovie" />

    <movie-list
      v-if="!hasSearchError"
      class="searchpanel__movie-list"
      v-on:movieSelected="setSelectedMovie"
      :searchResult="searchResult"
    />

    <div v-else class="searchpanel__error">{{searchErrorMsg}}</div>

    <pagination
      v-if="showPagination"
      :totalResults="totalResults"
      :pageNumber="pageNumber"
      v-on:pageChange="pageChange"
    />
  </div>
</template>

<script>
import MovieInput from "@/components/MovieInput.vue";
import MovieList from "@/components/MovieList.vue";
import Pagination from "@/components/Pagination.vue";

export default {
  name: "search-panel",
  components: { MovieInput, MovieList, Pagination },
  data() {
    return {
      lastSearchedMovieName: "",  // name of movie last searched in OMDB, used for app's internal use
      hasSearchError: false,      // if fetch api errors while searching for movie in omdb
      searchErrorMsg: "",         // error message from api 
      searchResult: [],           // list of movie from the api (max 10)
      totalResults: 0,            // total result for the search in OMDB, comes from api, used in Pagination component
      pageNumber: 1,              // current page number searched and displayed
      showPagination: false       // show pagination or not,  show pagination if total movie is more than 10
    };
  },
  methods: {
    async searchMovie(movieName) {
      this.hasSearchError = false;
      this.searchErrorMsg = "";
      let response = await fetch(
        `http://www.omdbapi.com/?apikey=${process.env.VUE_APP_OMDB_API_KEY}&s=${movieName}&type=movie&page=${this.pageNumber}`
      );
      let data = await response.json();
      if (data.Response === "True") {
        if (this.lastSearchedMovieName !== movieName) {
          this.$emit("displaySelectedMovieDetails", false);
          this.pageNumber = 1;
        }
        this.lastSearchedMovieName = movieName;
        this.searchResult = data.Search;
        this.totalResults = data.totalResults;
        this.showPagination = data.totalResults > 10;
      } else {
        this.hasSearchError = true;
        this.searchErrorMsg = data.Error || 'Something went wrong.';
        this.showPagination = false;
        this.$emit("displaySelectedMovieDetails", false);
        this.pageNumber = 1;
      }
    },
    pageChange(pageNumber) {
      this.pageNumber = pageNumber;
      this.searchMovie(this.lastSearchedMovieName);
    },
    setSelectedMovie(id) {
      this.$emit("movieSelected", id);
    }
  }
};
</script>

<style lang="scss">
.searchpanel {
  &__movie-list {
    margin-bottom: auto;
    flex: 1;
  }

  &__error {
    text-align: center;
    margin-top: 5rem;
    font-size: 2rem;
    color: red;
    min-width: 10rem;
  }
}
</style>
