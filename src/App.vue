<template>
  <div id="app">
    <!-- <AppHeader /> -->
    <header>
      <div class="logo">BoolFlix</div>
      <div class="search">
        <input
          v-on:keyup.enter="startSearch"
          class="btn btn-dark"
          type="text"
          placeholder="Cerca un film"
          v-model="cercaFilm"
        />
        <button @click="startSearch" class="btn btn-dark">
          <i class="fas fa-search"></i>
        </button>
      </div>
    </header>
    <main>
      <div class="container">
        <div class="card" v-for="(item, index) in searchMovies" :key="index">
          <h6>{{ index + 1 }}° risultato</h6>
          <h2>Titolo: {{ item.title }}</h2>
          <h3>Titolo originale:{{ item.original_title }}</h3>
          <h4>Lingua: {{ item.original_language }}</h4>
          <h4>Voto: {{ item.vote_average }}/10</h4>
        </div>
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
      cercaFilm: "",
    };
  },
  methods: {
    startSearch() {
      axios
        .get("https://api.themoviedb.org/3/search/movie", {
          params: {
            api_key: "c65d6e41ee9a7c6cdbdbaa81e45a6849",
            query: this.cercaFilm,
          },
        })
        .then((resp) => {
          this.searchMovies = resp.data.results;
        });
    },
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
  .card {
    border: 2px solid red;
    margin-bottom: 1rem;
    background-color: #141414;
  }
}
</style>
