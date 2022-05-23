<template>
  <div id="app">
    <!-- <AppHeader /> -->
    <header>
      <div class="logo">BoolFlix</div>
      <div class="search">
        <input class="btn btn-dark" type="text" placeholder="Cerca un film" />
        <button class="btn btn-dark">
          <i class="fas fa-search"></i>
        </button>
      </div>
    </header>
    <main>
      <div class="my-card" v-for="(item, index) in searchMovies" :key="index">
        <h6>{{ index + 1 }}° risultato</h6>
        <h2>Titolo: {{ item.title }}</h2>
        <h3>Titolo originale:{{ item.original_title }}</h3>
        <h4>Lingua: {{ item.original_language }}</h4>
        <h4>Voto: {{ item.vote_average }}/10</h4>
      </div>
    </main>
    <AppMain />
  </div>
</template>

<script>
// import AppHeader from "./components/AppHeader.vue";
import AppMain from "./components/AppMain.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    // AppHeader,
    AppMain,
  },
  data() {
    return {
      searchMovies: [],
    };
  },
  created() {
    axios
      .get("https://api.themoviedb.org/3/search/movie", {
        params: {
          api_key: "c65d6e41ee9a7c6cdbdbaa81e45a6849",
          query: "ritorno al futuro",
        },
      })
      .then((resp) => {
        this.searchMovies = resp.data.results;
      });
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";
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

main {
  background-color: #141414;
  font-family: sans-serif;
  color: white;
  .my-card {
    border: 2px solid red;
    width: 700px;
  }
}
</style>
