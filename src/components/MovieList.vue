<template>
    <div id="app">
        <h1>IMDB movies:</h1>
        <p> {{ msg }} </p>
    <div class="nav">
        <button @click="sortLowest()">Lowest rated</button>
        <button @click="sortHighest()">Highest rated</button>
        <button @click="sortName('asc')"> A-Z </button>
        <button @click="sortName('desc')"> Z-A </button>
    </div>
    <div class="nav">
        <input type="text" placeholder="Find in list..." v-model="find" @keyup="findMovies" />
        <p v-if="findMsg">{{ findMsg }}</p>
      </div>
    <div class="nav">
        <input type="text" placeholder="Search database..." v-model="search" @keypress.enter="searchMovies" />
        <button v-show="search" @click="searchMovies">Go!</button>
        <p v-if="movies && movies.length === 0">{{ searchMsg }}</p>
      </div>
    <div class="columns">
      <div class="col-1">
        <ul>
          <li v-for="movie in movies" :key="movie.id" >
            <Movie :movie="movie" :genres="genres" />
          </li>
        </ul>
      <div class="nav">
        current page: {{ page }}
        <button @click="fetchNextPage">next page</button>
      </div>
      </div>
      <div class="col-2">
        <GenreList :genres="genres.genres" :update="fetchGenreCollection"/>
      </div>
    </div>
</div>
</template>

<script>
import Movie from '@/components/Movie'
import GenreList from '@/components/GenreList'

export default {
  name: 'MovieList',
  components: { Movie, GenreList },
  data () {
    return {
      msg: 'Welcome. Happy viewing!',
      find: '',
      findMsg: '',
      search: '',
      searchMsg: 'Search returned zero results.',
      genres: [],
      backgroundLink: `http://image.tmdb.org/t/p/w500/gBmrsugfWpiXRh13Vo3j0WW55qD.jpg`,
      original: self.original,
      movies: self.movies,
      isActive: false,
      data: self.data,
      lastQuery: '',
      page: 1
    }
  },
  methods: {
    sortLowest () {
      this.movies.sort((a, b) => a.vote_average - b.vote_average)
    },
    sortHighest () {
      this.movies.sort((a, b) => b.vote_average - a.vote_average)
    },
    sortName (order) {
      this.movies.sort(function (a, b) {
        var titleA = a.title.toUpperCase()
        var titleB = b.title.toUpperCase()
        if ((titleA < titleB && order === 'asc') || (titleA > titleB && order === 'desc')) {
          return -1
        }
        if ((titleA < titleB && order === 'desc') || (titleA > titleB && order === 'asc')) {
          return 1
        }
        return 0
      })
    },
    updateList (result, query, nextPage) {
      let self = this
      self.movies = result.results.sort((a, b) => b.vote_average - a.vote_average)
      self.original = self.movies
      self.data = result
      self.lastQuery = query
      self.page = nextPage || 1
    },
    findMovies () {
      const newList = this.original.filter(movie => movie.title.toLowerCase().includes(this.find.toLowerCase()))
      this.movies = newList
      if (this.find.length === 0) {
        this.findMsg = ''
      } else if (this.movies.length === 0) {
        this.findMsg = 'There are no movies matching the current criteria'
      } else {
        this.findMsg = `There are ${newList.length} movies containing '${this.find}'.`
      }
    },
    searchMovies () {
      let self = this
      let query = `https://api.themoviedb.org/3/search/movie?api_key=d993bdf37f8ab7c574c990434a85a69f&language=en-US&query=${this.search.toLowerCase()}&page=1&include_adult=false
`
      fetch(`https://api.themoviedb.org/3/search/movie?api_key=d993bdf37f8ab7c574c990434a85a69f&language=en-US&query=${this.search.toLowerCase()}&page=1&include_adult=false
`).then(function (response) {
        return response.json()
      }).then(function (result) {
        self.updateList(result, query)
      })
    },
    fetchNextPage () {
      let self = this
      let nextPage = self.page + 1
      let query = self.lastQuery + '&page=' + nextPage
      fetch(query).then(function (response) {
        return response.json()
      }).then(function (result) {
        self.updateList(result, query, nextPage)
      })
    },
    fetchGenre () {
      fetch('https://api.themoviedb.org/3/genre/movie/list?api_key=d993bdf37f8ab7c574c990434a85a69f&language=en-US').then(function (response) {
        return response.json()
      }).then(function (result) {
        self.genres = result
      })
    },
    fetchGenreCollection (genreId) {
      let self = this
      let query = `https://api.themoviedb.org/3/genre/${genreId}/movies?api_key=d993bdf37f8ab7c574c990434a85a69f&language=en-US&include_adult=false&sort_by=created_at.asc%27`
      fetch(`https://api.themoviedb.org/3/genre/${genreId}/movies?api_key=d993bdf37f8ab7c574c990434a85a69f&language=en-US&include_adult=false&sort_by=created_at.asc%27`).then(function (response) {
        return response.json()
      }).then(function (result) {
        self.updateList(result, query)
      })
    }
  },
  mounted () {
    let self = this
    fetch('https://api.themoviedb.org/3/genre/movie/list?api_key=d993bdf37f8ab7c574c990434a85a69f&language=en-US').then(function (response) {
      return response.json()
    }).then(function (result) {
      self.genres = result
    })
    let query = 'https://api.themoviedb.org/3/discover/movie?api_key=d993bdf37f8ab7c574c990434a85a69f&sort_by=popularity.desc'
    fetch('https://api.themoviedb.org/3/discover/movie?api_key=d993bdf37f8ab7c574c990434a85a69f&sort_by=popularity.desc').then(function (response) {
      return response.json()
    }).then(function (result) {
      self.updateList(result, query)
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

.columns {
  display: flex;
  flex-direction: row;
  align-items: space-between;
}

.col-1 {
  flex: 3;
}

.col-2 {
  flex: 1;
}

@media screen and (max-width: 768px) {
  .columns {
    flex-direction: column;
  }
}

</style>
