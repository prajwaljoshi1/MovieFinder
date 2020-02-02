<template>
  <div class="movielist">
    <ul class="movielist__list">
      <li  v-for="(item, index) in searchResult" v-bind:key="index" class="movielist__item" v-on:click="selectItem(item)">
        <div class="movielist__item-title">{{item.Title}}</div>
        <div v-if="item.imdbID === selectedItemImdbId"  class="movielist__item-selected"><star class="svg"/></div>
        <div class="movielist__item-year">{{item.Year}} </div>
      </li>
    </ul>
  </div>
</template>

<script>
import Star from './icons/Star.vue';

export default {
  name: 'MovieList',
  components: { Star },
  props: {
    searchResult: Array
  },
  data() {
    return {
      selectedItemImdbId: ''
    };
  },
  methods: {
    selectItem(item) {
      this.selectedItemImdbId  = item.imdbID; 
      this.$emit("movieSelected", item.imdbID);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.selected {
  background-color: orange;
}
.movielist {
  &__list {
    list-style: none;
    padding: 4rem 0;
  }

  &__item {
    border: 2px solid #111;
    margin: 0.2rem;
    padding: .5rem 1rem;
    position: relative;
  }

  &__item-title {
    font-size: 1.4rem;
    font-weight: 600;
    text-align: left;
    margin-bottom: 1rem;
  }

  &__item-year {
    font-size: 1.2rem;
    text-align: right;
  }
  &__item-selected {
    position: absolute;
    right: 0.5rem;
    top: .5rem;
    
  }

  & .svg {
    fill: #444;
  }
}

</style>
