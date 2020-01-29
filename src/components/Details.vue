<template>
  <div id="offcanvas-details" uk-offcanvas="flip: true; overlay: true">
    <div class="uk-offcanvas-bar">
      <button class="uk-offcanvas-close" type="button" uk-close></button>
      <p v-if="loading">Loading...</p>
      <template v-else-if="movie && movie.Response ==='True'">
        <h3>{{ movie.Title }}</h3>

        <ul class="uk-tab" uk-tab>
          <li class="uk-active">
            <a href="#" uk-icon="icon: info"></a>
          </li>
          <li>
            <a href="#" uk-icon="icon: database"></a>
          </li>
          <li>
            <a href="#" uk-icon="icon: star"></a>
          </li>
        </ul>

        <ul class="uk-switcher uk-margin">
          <li>
            <img v-if="movie.Poster !== 'N/A'" :src="movie.Poster" style="max-width:150px" uk-img />
            <p>{{ movie.Plot }}</p>
            <p class="uk-text-warning">{{movie.Genre}}</p>
          </li>
          <li>
            <table class="uk-table uk-table-divider uk-table-small uk-table-justify">
              <tr>
                <td>Country:</td>
                <td>{{ movie.Country }}</td>
              </tr>
              <tr>
                <td>Year:</td>
                <td>{{ movie.Year }}</td>
              </tr>
              <tr>
                <td>Production:</td>
                <td>{{ movie.Production }}</td>
              </tr>
              <tr>
                <td>Director:</td>
                <td>{{ movie.Director }}</td>
              </tr>
              <tr>
                <td>Actors:</td>
                <td>{{ movie.Actors }}</td>
              </tr>
              <tr>
                <td>Runtime</td>
                <td>{{ movie.Runtime }}</td>
              </tr>
            </table>
          </li>

          <li>
            <table class="uk-table uk-table-divider uk-table-small uk-table-justify">
              <tr v-for="(item,index) in movie.Ratings" :key="index">
                <td>{{ item.Source }} :</td>
                <td>{{ item.Value }}</td>
              </tr>
            </table>
          </li>
        </ul>
        <p>
          <a :href="'https://www.imdb.com/title/'+movieId" target="_blank">
            <img
              src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/IMDb_logo.svg/450px-IMDb_logo.svg.png"
              style="max-width:50px;"
              uk-img
            />
          </a>
        </p>
      </template>
      <h3 v-else>Not found</h3>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Details",
  props: ["movieId", "apiKey"],
  data() {
    return {
      movie: null,
      loading: false
    };
  },
  watch: {
    movieId(val) {
      if (val) {
        this.getData();
      }
    }
  },
  computed: {
    genreArray() {
      return this.movie.Genre.split(",");
    }
  },
  methods: {
    getData() {
      this.loading = true;
      axios
        .get(
          "http://www.omdbapi.com/?i=" + this.movieId + "&apiKey=" + this.apiKey
        )
        .then(response => {
          this.movie = response.data;
        })
        .finally(() => {
          this.loading = false;
        });
    }
  }
};
</script>