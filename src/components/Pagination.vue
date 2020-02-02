<template>
  <div class="pagination">
    <button :disabled="disablePrevBtn" class="btn pagination_prev" v-on:click="prev()"> <arrow-left class="svg"/> </button>
    <div class="pagination__info">
        <div class="pagination__number">Page {{pageNumber}}</div>
        <div class="btn pagination__results">{{totalResults}} results</div>
        {{disablePrevBtn}}
    </div>
     <button :disabled="disableNextBtn" class="btn pagination_next" v-on:click="next()"> <arrow-right class="svg"/> </button>
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
  mounted() {
    this.disablePrevBtn = this.pageNumber <= 1;
  },
  data() {
    return{
      disableNextBtn: false,
      disablePrevBtn: false
    };
  },
  watch: {
    pageNumber(value){
      this.disablePrevBtn = value <= 1;
    }
  },
  methods: {
    next() {
      const totalResults = parseInt(this.totalResults);
      console.log("total pgnm ", totalResults/10 - 1 - this.pageNumber)// eslint-disable-line
      this.disableNextBtn = totalResults/10 - 1 - this.pageNumber <= 0;
      
      this.$emit("pageChange", this.pageNumber + 1);
    },
    prev() {
      const totalResults = parseInt(this.totalResults);
      this.disableNextBtn = totalResults/10 - 1 - this.pageNumber <= 0; // Math.floor(totalResults/10) - 1 < this.pageNumber;

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

  &__info {
    padding-top: 2rem;
  }

  & .svg {
    fill: #444;
  }

  & .btn {
    border: none;
    background-color: transparent;

    &:disabled {
      opacity: 0.5;
    }
  }
}
</style>
