<template>
<div>
  <div class="movie-header-outer">
    <div class="movie-header-inner">
      <img :src="imageLink" class="thumbnail"/>
      <div class="title-and-score">
        <h3>{{ movie.title }}</h3>
        <br>
        <span :class="{ popular: movie.vote_average > 8.2 }"> Rating: {{ movie.vote_average }} </span>
      </div>
    </div>
    <button @click="show = !show" class="smallbutton">show overview</button>
  </div>
        <div id="rating-bar" :style="styleObject"></div>
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
  props: ['movie'],
  data () {
    return {
      show: false,
      imageLink: `http://image.tmdb.org/t/p/w185//${this.movie.poster_path}`,
      backgroundLink: `http://image.tmdb.org/t/p/w500/${this.movie.backdrop_path}`,
      styleObject: {
        color: '#fff',
        height: '2px',
        width: `${this.movie.vote_average * 10}%`,
        backgroundImage: `linear-gradient(90deg, rgba(25, 25, 100, ${this.movie.vote_average / 10}), white)`
      }
    }
  },
  mounted () {

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
  background-color: rgba(204, 204, 204, 0.4);
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

.thumbnail{
  width: 50px;
  height: 70px;
  border: 1px solid white;
  box-shadow: 0 0 5px black;
}

.image {
  opacity: 0.8;
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
  align-items: center;
  overflow: scroll;
  background: rgba(50, 60, 75, 0.7);
  color: white;
  text-align: justify;
}

.smallbutton {
  font-size: 10px;
}

.popular {
  background-color: rgba(255, 188, 63, 0.856);
}
</style>
