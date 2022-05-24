<template>
  <div class="col-12 col-md-6 col-lg-4 col-xl-3 mb-4">
    <div class="card rounded">
      <!-- se la locandina è presente la mostro -->
      <img
        v-if="movie.poster_path !== null"
        class="card-img-top"
        :src="`https://www.themoviedb.org/t/p/w342${movie.poster_path}`"
      />

      <!-- altrimenti mostro un'immagine generica -->
      <img
        v-else
        class="card-img-top"
        src="https://bitsofco.de/content/images/2018/12/broken-1.png"
      />

      <div class="card-body">
        <h3 class="card-title mt-2">{{ movie.title }} {{ movie.name }}</h3>
        <!-- il titolo originale viene mostrato solo se è diverso dal titolo, sia per i film che per le serie -->
        <h5
          v-if="
            movie.original_title !== movie.title ||
            movie.original_name !== movie.name
          "
          class="card-subtitle text-muted mb-2"
        >
          Titolo Originale: {{ movie.original_title }} {{ movie.original_name }}
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
          <!-- altrimenti si fa un ciclo v-for che aggiunge tante stelle quanto il numero arrotondato che esce dalla funzione starsVote -->
          <span v-else>
            <!-- stelle piene -->
            <i
              v-for="(item, index) in starsVote(movie.vote_average)"
              :key="index"
              class="fas fa-star"
            ></i>
            <!-- stelle vuote -->
            <i
              v-for="(item, index) in starsTotal"
              :key="index"
              class="far fa-star"
            ></i>
          </span>
        </h6>
        <!-- mostra la descrizione del film -->
        <small> Trama: {{ movie.overview }} </small>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppCard",
  data() {
    return {
      // array con le bandierine delle lingue più popolari
      langFlags: ["cn", "de", "en", "es", "fr", "it", "ja", "ru", "pt"],
    };
  },
  // prelevo l'oggetto dal genitore
  props: {
    movie: Object,
  },
  methods: {
    // prende il voto espresso da 1 a 10 e lo divide per 2 e poi arrotonda per eccesso
    starsVote(vote) {
      return Math.ceil(vote / 2);
    },
    // sottrae da 5 il numero di stelle gialle e mostra le rimanenti
    starsTotal() {
      return 5 - this.starsVote();
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
  cursor: pointer;

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
  }

  &:hover .card-body {
    display: block;
    background-color: rgba(0, 0, 0, 0.8);
  }
}
</style>
