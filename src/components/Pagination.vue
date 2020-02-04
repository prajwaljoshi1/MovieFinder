<template>
  <div class="pagination">
    <button :disabled="disablePrevBtn" class="btn pagination_prev" v-on:click="prev()">
      <arrow-left class="svg" />
    </button>
    <div class="pagination__info">
      <div class="pagination__number">Page {{pageNumber}}</div>
      <div class="pagination__results">{{totalResults}} results</div>
    </div>
    <button :disabled="disableNextBtn" class="btn pagination_next" v-on:click="next()">
      <arrow-right class="svg" />
    </button>
  </div>
</template>

<script>
import ArrowLeft from "./icons/ArrowLeft.vue";
import ArrowRight from "./icons/ArrowRight.vue";

export default {
  name: "Pagination",
  components: { ArrowLeft, ArrowRight },
  props: {
    totalResults: String,
    pageNumber: Number
  },
  mounted() {
    this.disablePrevBtn = this.pageNumber <= 1;
  },
  data() {
    return {
      disableNextBtn: false,    // disable next btn, when next page is not applicable
      disablePrevBtn: false     // disable prev btn, when previous page is not applicable
    };
  },
  watch: {
    pageNumber(value) {
      this.disablePrevBtn = value <= 1;
    }
  },
  methods: {
    next() {
      const totalResults = parseInt(this.totalResults);
      // need to disable the 'next' button  checked at every next click and button is disabled for subsequent time if appliable.
      this.disableNextBtn = totalResults / 10 - 1 - this.pageNumber <= 0;

      this.$emit("pageChange", this.pageNumber + 1);
    },
    prev() {
      this.disableNextBtn = false;
      this.$emit("pageChange", this.pageNumber - 1);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.pagination {
  display: flex;
  justify-content: space-between;

  &__info {
    padding-top: 1.5rem;
    text-align: center;
  }
  &__number,
  &__results {
    font-size: 1.2rem;
    font-weight: 600;
  }

  & .svg {
    fill: #0072ff;
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
