<template>
    <div id="genrelist">
      <h2> genres</h2>
      <div class="nav">
        <button v-for="genre in genres" :key="genre.id" @click="update(genre.id)">{{ genre.name }}</button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'GenreList',
  props: ['genres', 'update']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nav {
  display: inline-flex;
  flex-direction: column;
}
.nav button {
  margin: 2px;
}
</style>
