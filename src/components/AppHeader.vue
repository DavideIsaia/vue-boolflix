<template>
  <header class="d-flex justify-content-between">
    <img class="logo" src="../assets/logo.png" />
    <div class="search">
      <!-- fa partire la ricerca al rilascio del tasto invio o del bottone e passa il risultato tramite $emit al genitore -->
      <input
        v-on:keyup.enter="$emit('searchMovies', cercaFilm)"
        class="btn btn-dark"
        type="text"
        placeholder="Cerca film/serie e premi Invio"
        v-model="cercaFilm"
      />
      <button
        @click="$emit('searchMovies', cercaFilm)"
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
  data() {
    return {
      cercaFilm: "",
    };
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
