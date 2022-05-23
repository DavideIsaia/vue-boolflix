<template>
  <header>
    <div class="logo">BoolFlix</div>
    <div class="search">
      <input type="text" placeholder="Cerca un film" v-model="cercaFilm" />
      <button><i class="fas fa-search"></i></button>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
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
    padding: 1rem;
  }
}
</style>
