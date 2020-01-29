<template>
  <div class="home">
    <template v-if="searchTerm.apiKey">
    <UkSearch @submit="makeSearch" />
    <section class="uk-section">
      <div class="uk-container">
        <template v-if="hasResults">
          <SearchResult :movies="searchResponse.Search" @selected="showDetails" />
          <Pagination :total-items="parseInt(searchResponse.totalResults)" @change="setPage" />
          <Details :movie-id="movieId" :api-key="searchTerm.apiKey" />
        </template>
        <div v-else-if="searchResponse" uk-alert>
          <b>{{ searchTerm }}</b> not found
        </div>
      </div>
    </section>
    </template>
    <div class="uk-container" v-else>
      <div class="uk-alert">
        <h3>No api key</h3>
        <p>
          Please get an api key from 
          <a href="http://www.omdbapi.com/" target="_blank">OMDb API</a>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import UkSearch from "@/components/UkSearch.vue";
import SearchResult from "@/components/SearchResult.vue";
import Pagination from "@/components/Pagination.vue";
import Details from "@/components/Details.vue";

export default {
  name: "home",
  components: {
    UkSearch,
    SearchResult,
    Pagination,
    Details
  },
  data() {
    return {
      searchTerm: {
        apiKey: null, // your OMDb apikey
        s: null, // Movie title to search for
        type: null, // Type of result to return [movie, series, episode]
        y: null, // Year of release
        r: null, // The data type to return.[json, xml]. default json
        page: null, // Page number to return. [1-100]. default 1
        callback: null, //JSONP callback name
        v: null //API version (reserved for future use). default 1
      },
      movieId: null,
      searchResponse: null
    };
  },
  computed: {
    hasResults() {
      // if search has result
      if (this.searchResponse && this.searchResponse.Response === "True") {
        return true;
      }

      return false;
    }
  },
  methods: {
    makeSearch(val) {
      // api endpoint
      let url = "http://www.omdbapi.com/?";

      if (val) {
        this.searchTerm.s = val;
      }

      // convert searchTerm object to query string
      Object.keys(this.searchTerm).forEach(key => {
        if (this.searchTerm[key] !== null) {
          url += key + "=" + this.searchTerm[key] + "&";
        }
      });

      axios.get(url).then(response => {
        this.searchResponse = response.data;
      });
    },
    setPage(val) {
      this.searchTerm.page = val;
      this.makeSearch();
    },
    showDetails(val) {
      this.movieId = val;

      // show UiKit off-canvas
      window.UIkit.offcanvas("#offcanvas-details").show();
    }
  }
};
</script>
