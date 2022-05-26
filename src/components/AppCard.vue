<template>
  <div class="col-12 col-md-6 col-lg-4 col-xl-3 mb-4">
    <div @mouseleave="showDetails = false" class="card rounded">
      <!-- se la locandina è presente la mostro -->
      <img
        v-if="movie.poster_path !== null"
        class="card-img-top"
        :src="`https://www.themoviedb.org/t/p/w342${movie.poster_path}`"
      />

      <!-- altrimenti mostro un'img generica -->
      <img v-else src="../assets/no-image.png" alt="" />

      <div class="card-body">
        <h3 class="card-title mt-2">{{ movie.title || movie.name }}</h3>
        <!-- il titolo originale viene mostrato solo se è diverso dal titolo, sia per i film che per le serie -->
        <h5
          v-if="
            movie.original_title !== movie.title ||
            movie.original_name !== movie.name
          "
          class="card-subtitle text-muted mb-2"
        >
          Titolo Originale: {{ movie.original_title || movie.original_name }}
        </h5>
        <!-- se la lingua non è inclusa nell'array di bandiere, mostra solo la sigla della lingua -->
        <h6 class="card-text">
          Lingua:
          <span v-if="!langFlags.includes(movie.original_language)">
            {{ movie.original_language }}
          </span>

          <img
            v-else
            class="flag"
            :src="require(`../assets/flags/${movie.original_language}.png`)"
            :alt="movie.original_language"
          />
        </h6>
        <!-- se il voto è 0 non vengono visualizzate le stelle -->
        <h6 class="card-text">
          Voto:
          <span v-if="movie.vote_average == 0">Non Pervenuto</span>
          <!-- altrimenti si fa un ciclo v-for che tramitre operatore ternario aggiunge la classe delle stelle piene e vuote -->
          <span v-else>
            <i
              v-for="(item, index) in 5"
              :key="index"
              class="fa-star"
              :class="index < starsVote() ? 'fas' : 'far'"
            ></i>
          </span>
        </h6>
        <!-- mostra le informazioni extra -->
        <div class="info rounded" @click="castAndGenres(movie)">
          <h6 class="text-uppercase text-center pt-2">
            Clicca qui per maggiori info
          </h6>
          <div class="details rounded" v-if="showDetails">
            <div class="mt-2 mb-1">
              Cast:
              <div
                class="details__small"
                v-for="(person, index) in castArray"
                :key="index"
              >
                {{ person.original_name }}
              </div>
            </div>
            <div class="mt-2 mb-1">
              Genere:
              <div
                class="details__small"
                v-for="(genre, index) in genresArray"
                :key="index"
              >
                {{ genre.name }}
              </div>
            </div>
          </div>
        </div>
        <!-- mostra la descrizione del film, se è presente -->
        <h6 v-if="movie.overview">
          Trama: <small>{{ movie.overview }}</small>
        </h6>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AppCard",
  data() {
    return {
      // array con le bandierine delle lingue più popolari
      langFlags: ["cn", "de", "en", "es", "fr", "it", "ja", "ru", "pt"],
      // array per cast e generi
      castArray: [],
      genresArray: [],
      showDetails: false,
    };
  },
  // prelevo l'oggetto dal genitore
  props: {
    movie: Object,
  },
  methods: {
    // prende il voto espresso da 1 a 10 e lo divide per 2 e poi arrotonda per eccesso
    starsVote() {
      return Math.ceil(this.movie.vote_average / 2);
    },

    // metto la chiamata axios direttamente dentro questo componenete in quanto è indipendente dalla query che viene cercata nell'header
    castAndGenres(movie) {
      const params = {
        api_key: "c65d6e41ee9a7c6cdbdbaa81e45a6849",
        language: "it-IT",
      };
      // faccio le chiamate axios e aspetto che arrivino le risposte tutte insieme
      const castRequest = axios.get(
        `https://api.themoviedb.org/3/movie/${movie.id}/credits`,
        { params }
      );
      const genreRequest = axios.get(
        `https://api.themoviedb.org/3/movie/${movie.id}`,
        { params }
      );
      // prelevo il genere e nella stessa chiamata aggiungo lo switch per mostrare/nascondere i risultati al clic
      axios.all([castRequest, genreRequest]).then((resp) => {
        // prelevo solo i primi 5 risultati tramite splice
        this.castArray = resp[0].data.cast.splice(0, 5);
        this.genresArray = resp[1].data.genres;
        this.showDetails = !this.showDetails;
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  position: relative;
  background-color: #060606;
  width: 100%;
  height: 100%;

  img {
    margin: auto;
  }

  .flag {
    height: 1.5rem;
  }

  .fa-star {
    display: inline;
    margin-left: 0.1rem;
    color: gold;
  }

  .card-body {
    display: none;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;

    .info {
      background-color: #e50914;
      position: relative;
      padding: 0.1rem 0.3rem;
      user-select: none;
      cursor: pointer;

      .details {
        position: absolute;
        top: 2.4rem;
        left: 1.5rem;
        padding: 1rem 3rem;
        background-color: rgba(229, 9, 20, 0.9);
        box-shadow: 3px 3px 10px black;
        &__small {
          font-size: 0.8rem;
        }
      }
    }
  }

  &:hover .card-body {
    display: block;
    background-color: rgba(0, 0, 0, 0.8);
  }
}
</style>
