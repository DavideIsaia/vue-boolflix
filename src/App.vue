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
        // chiave personale necessaria per fare le richieste all'API
        api_key: "c65d6e41ee9a7c6cdbdbaa81e45a6849",
        // campo di ricerca obbligatorio per mostrare i risultati
        query: cercaFilm,
        // lingua in cui vengono mostrate le locandine e le descrizioni dei film (opzionale)
        language: "it-IT",
      };

      // faccio le chiamate axios e aspetto che arrivino le risposte tutte insieme
      const moviesRequest = axios.get(
        "https://api.themoviedb.org/3/search/movie",
        { params }
      );
      const seriesRequest = axios.get(
        "https://api.themoviedb.org/3/search/tv",
        { params }
      );

      axios.all([moviesRequest, seriesRequest]).then((resp) => {
        this.moviesArray = resp[0].data.results;
        this.seriesArray = resp[1].data.results;
      });
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";
</style>
