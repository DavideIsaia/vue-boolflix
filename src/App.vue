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
      genreFilterMovies: [],
      genreFilterSeries: [],
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
          // come risposta axios, passo l'array filtrato per genere
          this.genreFilterMovies = resp[0].data.results;
          // se non sono stati selezionati generi, passo normalmente l'array completo
          this.moviesArray = this.genreFilterMovies;
          this.genreFilterSeries = resp[1].data.results;
          this.seriesArray = this.genreFilterSeries;
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

    // filtro i generi presenti dentro ogni elemento della ricerca e se il genere combacia con quello della option del select, allora lo mostro
    filterContents(searchId) {
      if (searchId !== "") {
        this.moviesArray = this.genreFilterMovies.filter((element) => {
          // se l'array degli id di genere contiene elementi, allora ciclo for per vedere se qualcuno combacia con quello cercato
          if (element.genre_ids.length > 0) {
            for (let i = 0; i < element.genre_ids.length; i++) {
              if (element.genre_ids[i] == searchId) {
                return true;
              }
            }
          }
        });
        // altrimenti mostro gli elementi normalmente
      } else {
        this.moviesArray = this.genreFilterMovies;
      }
      // ripeto la stessa cosa per le serie tv
      if (searchId !== "") {
        this.seriesArray = this.genreFilterSeries.filter((element) => {
          if (element.genre_ids.length > 0) {
            for (let i = 0; i < element.genre_ids.length; i++) {
              if (element.genre_ids[i] == searchId) {
                return true;
              }
            }
          }
        });
      } else {
        this.seriesArray = this.genreFilterSeries;
      }
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";
</style>
