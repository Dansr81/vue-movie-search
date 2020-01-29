<template>
    <div class="uk-margin">
        <ul class="uk-pagination uk-flex-center">
            <li v-if="currentPage>1">
                <a href="#" @click.prevent="clickEvent(currentPage-1)"><span uk-pagination-previous></span></a>
            </li>
            <li :class="{'uk-active': currentPage === item}" 
                v-for="item in pages" 
                :key="item"
                @click.prevent="clickEvent(item)">
                <a href="#">{{ item }}</a>
            </li>
             <li v-if="currentPage<totalPages">
                <a href="#" @click.prevent="clickEvent(currentPage+1)"><span uk-pagination-next></span></a>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
  name: "Pagination",
  props: {
    totalItems: Number
  },
  data() {
    return {
      currentPage: 1, // initial page
      pageSize: 10, // api return always 10 items
      totalPages: null,
      startPage: null,
      endPage: null,
      startIndex: null,
      endIndex: null,
    };
  },
  computed:{
      pages(){
          return new Array(1+this.endPage - this.startPage)
            .fill()
            .map((d, i) => i + this.startPage);
      }
  },
  watch:{
      currentPage(){
          this.setPager();
      }
  },
  mounted(){
      this.setPager();
  },
  methods: {
    clickEvent(val){
      this.$emit("change",val);
      this.currentPage = val;
    },
    setPager() {
      // calculate total pages
      this.totalPages = Math.ceil(this.totalItems / this.pageSize);

      if (this.totalPages <= 10) {
        // less than 10 total pages so show all
        this.startPage = 1;
        this.endPage = this.totalPages;
      } else {
        // more than 10 total pages so calculate start and end pages
        if (this.currentPage <= 6) {
          this.startPage = 1;
          this.endPage = 10;
        } else if (this.currentPage + 4 >= this.totalPages) {
          this.startPage = this.totalPages - 9;
          this.endPage = this.totalPages;
        } else {
          this.startPage = this.currentPage - 5;
          this.endPage = this.currentPage + 4;
        }
      }

      // calculate start and end item indexes
      this.startIndex = (this.currentPage - 1) * this.pageSize;
      this.endIndex = Math.min(this.startIndex + this.pageSize - 1, this.totalItems - 1);
    }
  }
};
</script>

<style>
</style>