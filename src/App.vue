<template>
  <div id="app">
    <AppHeader @searchMovies="startSearch" />
    <AppMain :moviesArray="moviesArray" :seriesArray="seriesArray" />
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
import AppMain from "./components/AppMain.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    AppHeader,
    AppMain,
  },
  data() {
    return {
      moviesArray: [],
      seriesArray: [],
    };
  },
  methods: {
    startSearch(cercaFilm) {
      const params = {
        api_key: "c65d6e41ee9a7c6cdbdbaa81e45a6849",
        query: cercaFilm,
      };

      axios
        .get("https://api.themoviedb.org/3/search/movie", { params })
        .then((resp) => {
          this.moviesArray = resp.data.results;
        });

      axios
        .get("https://api.themoviedb.org/3/search/tv", { params })
        .then((resp) => {
          this.seriesArray = resp.data.results;
        });
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";
</style>
