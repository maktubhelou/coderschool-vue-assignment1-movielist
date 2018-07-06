<template>
<div>
  <div>
  {{ movie.title }} |<span> {{ movie.vote_average }} </span> |
  <button @click="show = !show">show overview</button>

  </div>
  <div v-show="show">
    {{movie.overview}}
  </div>
</div>
</template>

<script>
export default {
  name: 'Movie',
  props: ['movie'],
  data () {
    return {
      show: false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
