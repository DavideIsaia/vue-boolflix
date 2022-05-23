<template>
  <header>
    <div class="logo">BoolFlix</div>
    <div class="search">
      <input
        class="btn btn-dark"
        type="text"
        placeholder="Cerca un film"
        v-model="cercaFilm"
      />
      <button @click="startSearch" class="btn btn-dark">
        <i class="fas fa-search"></i>
      </button>
    </div>
    <div
      class="search-results"
      v-for="(item, index) in searchMovies"
      :key="index"
    >
      <div>{{ index + 1 }}</div>
      <h2>{{ item.title }}</h2>
      <h3>{{ item.original_title }}</h3>
      <h4>{{ item.original_language }}</h4>
      <h5>{{ item.vote_average }}</h5>
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
  display: flex;
  background-color: #060606;
  justify-content: space-between;

  .logo {
    color: red;
    text-transform: uppercase;
    font-size: 3rem;
    padding: 1rem;
  }

  .search {
    padding: 2rem 1rem;
  }
}
</style>
