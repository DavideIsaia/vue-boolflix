<template>
  <div id="app">
    <AppHeader
      @searchMovies="startSearch"
      @getSelectedGenre="filterContents"
      :genresArray="genresArray"
    />
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
      genresArray: [],
      mainMovies: [],
      mainSeries: [],
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
      const moviesGenresRequest = axios.get(
        "https://api.themoviedb.org/3/genre/movie/list",
        { params }
      );
      const seriesGenresRequest = axios.get(
        "https://api.themoviedb.org/3/genre/tv/list",
        { params }
      );

      axios
        .all([
          moviesRequest,
          seriesRequest,
          moviesGenresRequest,
          seriesGenresRequest,
        ])
        .then((resp) => {
          this.moviesArray = resp[0].data.results;
          this.seriesArray = resp[1].data.results;
          // per adesso svolgo i vari passaggi dentro il then, mettendo in un array i generi dei film
          const moviesGenresArray = resp[2].data.genres;
          const movieGenresId = function () {
            const finalResult = [];
            moviesGenresArray.forEach((element) => {
              finalResult.push(element.id);
            });
            return finalResult;
          };
          // svolgo gli stessi passaggi anche per le serie tv e concateno i risultati nello stesso array dei film
          const seriesGenresArray = resp[3].data.genres;
          const filteredSeriesGenres = seriesGenresArray.filter((element) => {
            const filteredArray = movieGenresId();
            if (!filteredArray.includes(element.id)) {
              return true;
            }
          });
          // risultato finale
          this.genresArray = moviesGenresArray.concat(filteredSeriesGenres);
        });
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";
</style>
