<template>
    <div id="app">
    <h1>IMDB movies:</h1>
    <p> {{ msg }} </p>
    <div class="nav">
        <button @click="sortLowest()">Lowest rated</button>
        <button @click="sortHighest()">Highest rated</button>
    </div>
    <ul>
        <li>
           <strong> Movie name | <span>Movie rating</span> </strong>
        </li>
        <li v-for="(movie, index) in movies" :key="index">
          <Movie :movie="movie"/>
        </li>
    </ul>
</div>
</template>

<script>
import Movie from '@/components/Movie'

export default {
  name: 'MovieList',
  components: { Movie },
  data () {
    return {
      msg: 'Welcome to Vue Movie List. Enjoy the App!',
      movies: self.movies,
      isActive: false
    }
  },
  methods: {
    sortLowest () {
      this.movies.sort((a, b) => a.vote_average - b.vote_average)
    },
    sortHighest () {
      this.movies.sort((a, b) => b.vote_average - a.vote_average)
    }
  },
  mounted () {
    let self = this
    fetch('https://api.themoviedb.org/3/discover/movie?api_key=d993bdf37f8ab7c574c990434a85a69f&sort_by=popularity.desc').then(function (response) {
      return response.json()
    }).then(function (result) {
      self.movies = result.results.sort((a, b) => b.vote_average - a.vote_average)
    })
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
