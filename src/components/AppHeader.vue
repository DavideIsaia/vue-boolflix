<template>
  <header class="d-flex justify-content-between">
    <img class="logo d-none d-lg-inline-block" src="../assets/logo.png" />
    <div class="search">
      <!-- aggiungo un select per scegliere un genere per filtrare film e serie tv -->
      <select
        class="btn btn-dark"
        name="genres"
        id="genres"
        v-model="selectedGenre"
        @change="$emit('getSelectedGenre', selectedGenre)"
      >
        <option value="" selected>Seleziona un genere</option>
        <option
          v-for="element in genresArray"
          :key="element.id"
          :value="element.id"
        >
          {{ element.name }}
        </option>
      </select>
      <!-- fa partire la ricerca al rilascio del tasto invio o del bottone e passa il risultato tramite $emit al genitore -->
      <input
        v-on:keyup.enter="sendSearchEvent(), (selectedGenre = '')"
        class="btn btn-dark"
        type="text"
        placeholder="Cerca film/serie e premi Invio"
        v-model="cercaFilm"
      />
      <button
        @click="sendSearchEvent(), (selectedGenre = '')"
        class="btn btn-dark d-none d-md-inline-block"
      >
        <i class="fas fa-search"></i>
      </button>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
  props: {
    genresArray: Array,
  },
  data() {
    return {
      cercaFilm: "",
      selectedGenre: "",
    };
  },
  methods: {
    // evita di mandare richieste vuote ad axios
    sendSearchEvent() {
      if (this.cercaFilm.trim()) {
        this.$emit("searchMovies", this.cercaFilm);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 100;
  background: linear-gradient(0deg, rgba(6, 6, 6, 0) 0%, rgba(6, 6, 6, 1) 100%);

  .logo {
    padding: 1rem;
  }

  .search {
    padding: 2rem 1rem;
  }
}
</style>
