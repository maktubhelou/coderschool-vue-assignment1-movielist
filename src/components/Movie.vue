<template>
<div>
  <div class="movie-header-outer" :style="mainBackground">
    <div class="movie-header-inner">
      <img :src="imageLink" class="thumbnail"/>
      <div class="title-and-score">
        <h3>{{ movie.title }}</h3>
        <br>
        <span class="rating" :class="{ popular: movie.vote_average > 8.2 }"> Rating: {{ movie.vote_average }} </span>
      </div>
    </div>
    <div class="right">
      <div class="badge badge-purple">released: {{ movie.release_date }}</div>
      <div class="badge badge-purple">votes: {{ movie.vote_count }}</div>
      <div class="badge badge-purple">language: {{ movie.original_language.toUpperCase() }}</div>
      <div>
        <p v-for="value in movieGenres" :key="value.id" class="badge badge-orange">{{ value.name }}</p>
      </div>
    <button @click="show = !show" class="smallbutton badge">show overview</button>
    </div>
  </div>
        <div id="rating-bar" :style="ratingBar"></div>
  <div v-show="show" class="movie-box">
    <img :src="backgroundLink" class="image"/>
    <div class="overview">
    {{ movie.overview }}
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Movie',
  props: ['movie', 'genres'],
  data () {
    return {
      show: false,
      imageLink: `http://image.tmdb.org/t/p/w185//${this.movie.poster_path}`,
      backgroundLink: `http://image.tmdb.org/t/p/w500/${this.movie.backdrop_path}`,
      movieGenres: [],
      ratingBar: {
        color: '#fff',
        height: '2px',
        width: `${this.movie.vote_average * 10}%`,
        backgroundImage: `linear-gradient(90deg, rgba(25, 25, 100, ${this.movie.vote_average / 10}), white)`
      },
      mainBackground: {
        backgroundImage: `linear-gradient(90deg, rgba(200, 200, 250, ${this.movie.vote_average / 10}), white)`
      }
    }
  },
  methods: {
    getMovieGenres () {
      let newGenres = []
      let cardMovieGenres = [...this.movie.genre_ids]
      let allGenres = [...this.genres.genres]
      cardMovieGenres.forEach((genre) => {
        let newGenre = allGenres.filter(function (obj) {
          return obj.id === genre
        })
        newGenres.push(newGenre[0])
      })
      this.movieGenres = [...newGenres]
    }
  },
  mounted () {
    this.getMovieGenres()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.movie-header-outer {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 5px;
  box-shadow: 0 0 5px 5px rgba(255, 255, 255, 0.5);
}

.movie-header-inner {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}

.movie-box {
  display: flex;
  margin-top: 10px;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  position: relative;
  width: 100%;
}

.badge {
  display: inline-flex;
  font-size: 9px;
  padding: 0px 8px;
  margin-top: 1px;
  border: .5px solid transparent;
  border-radius: 10px;
  margin-left: 2px;
}

.badge-orange {
  border: .5px solid rgba(255, 188, 63, 0.856);
}

.badge-purple {
  border: 0.5px solid transparent;
  color: rgb(255, 255, 255);
  background: rgba(101, 29, 184, 0.356);
}

.thumbnail{
  width: 50px;
  height: 70px;
  border: 1px solid white;
  box-shadow: 0 0 5px black;
}

.image {
  opacity: 0.2;
  width: 90%;
  height: 90%;
  margin-top: 5px;
  position: relative;
  filter: blur(5px);
  border: 5px solid white;
}

.title-and-score {
  text-align: left;
  margin-left: 10px;
}

.overview {
position: absolute;
    height: 95%;
    width: 90%;
    padding: 15px;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    overflow: scroll;
    background: rgba(39, 12, 122, 0.4);
    color: white;
    text-align: justify;
}

.right, .smallbutton {
  font-size: 10px;
}

.right {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-end;
}

.rating {
  border-radius: 10px;
  padding: 0 10px;
  background-color: rgba(243, 207, 142, 0.6);
}

.popular {
  color: rgb(255, 255, 255);
  background-color: rgba(231, 60, 12, 0.856);
}
</style>
