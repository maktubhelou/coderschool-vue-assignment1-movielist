<template>
    <div id="app">
    <h1>IMDB movies:</h1>
    <p> {{ msg }} </p>
    <div class="nav">
        <button>Lowest rated</button>
        <button>Highest rated</button>
    </div>
    <ul>
        <li>
            Movie name <span>Movie rating</span>
        </li>
    </ul>
</div>
</template>

<script>
export default {
  name: 'MovieList',
  data () {
    return {
      msg: 'Welcome to Vue Movie List. Enjoy the App!'
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
