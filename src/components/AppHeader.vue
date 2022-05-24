<template>
  <header class="d-flex justify-content-between">
    <div class="logo">BoolFlix</div>
    <div class="search">
      <input
        v-on:keyup.enter="$emit('searchMovies', cercaFilm)"
        class="btn btn-dark"
        type="text"
        placeholder="Cerca un film"
        v-model="cercaFilm"
      />
      <button @click="$emit('searchMovies', cercaFilm)" class="btn btn-dark">
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
  background-color: #060606;

  .logo {
    color: #e50914;
    text-transform: uppercase;
    font-size: 3rem;
    padding: 1rem;
  }

  .search {
    padding: 2rem 1rem;
  }
}
</style>
