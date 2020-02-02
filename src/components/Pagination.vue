<template>
  <div class="pagination">
    <div class="pagination_prev" v-on:click="prev()"> <arrow-left class="svg"/> </div>
    <div class="pagination__info">
        <div class="pagination__number">Page {{pageNumber}}</div>
        <div class="btn pagination__results">{{totalResults}} results</div>
    </div>
     <div class="btn pagination_next" v-on:click="next()"> <arrow-right class="svg"/> </div>
  </div>
</template>

<script>
import ArrowLeft from './icons/ArrowLeft.vue';
import ArrowRight from './icons/ArrowRight.vue';

export default {
  name: 'Pagination',
  components: { ArrowLeft, ArrowRight },
  props: {
    totalResults: String,
    pageNumber: Number
  },
  data() {
    return{
    };
  },
  watch: {
    // totalResults(value) {

    // }
  },
  methods: {
    next() {
      this.$emit("pageChange", this.pageNumber + 1);
    },
    prev() {
      this.$emit("pageChange", this.pageNumber - 1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.pagination {
  display: flex;
  justify-content: space-between;

  & .svg {
    fill: #444;
  }


}
</style>
