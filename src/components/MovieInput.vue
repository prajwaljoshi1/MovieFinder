<template>
  <div class="movieinput">
    <input v-model="movieName" class="movieinput__input" type="text" placeholder="Search movies" autofocus />
  </div>
</template>

<script>
import debounce from "@/helpers/debounce.js";

export default {
  name: "MovieInput",
  data() {
    return {
      movieName: ""      // searched movie name
    };
  },
  watch: {
    movieName: debounce(function(newVal, oldVal) {
      if (newVal.length > 0 && newVal !== oldVal) {
        this.$emit("movieTitleChange", newVal.trim());
      }
    }, 250)
  }
};
</script>


<style lang="scss">
.movieinput {
  &__input {
    font-size: 2rem;
    color: #111;
    width: 100%;
    padding: 2rem 2rem;
    border: 2px solid #111;
    border-radius: 8px;
  }
}
</style>
