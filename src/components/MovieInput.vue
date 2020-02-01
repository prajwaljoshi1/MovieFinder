<template>
  <div class="movieinput">
   <input v-model.lazy="movieName" class="movieinput__input" type="text" placeholder="Search movies">
  </div>
</template>

<script>
export default {
  name: 'MovieInput',
  data() {
      return {
          movieName: ''
      };
  },
  watch: {
    movieName(newVal, oldVal) {
      newVal = newVal.trim();
      if (newVal.length > 1 && newVal !== oldVal) {
        this.$emit("movieTitleChange", newVal);
      }
    }
  }
}
</script>


<style lang="scss">
.movieinput {
  &__input {
    font-size: 2rem;
    color: #111;
    width: 100%;
    padding: 2rem 2rem;
    border: 2px solid  #111;
    border-radius: 8px;
  }
}
</style>
