<template lang="pug">
  #app
    h1 PlatziMusic
    spinner(v-show="loading")
    select(v-model="selectCountry")
      option(v-for="country in countries" :value="country.value") {{ country.name }}
    ul
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>



<script>

import getArtists from './api'
import artist from './components/artist.component.vue'
import spinner from './components/spinner.component.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Peru', value: 'peru'},
        {name: 'España', value: 'spain'},
        {name: 'Argentina', value: 'argentina'}
      ],
      selectCountry: 'peru',
      loading: true
    }
  },
  components: {
    artist, spinner
  },
  methods: {
    refreshArtist(){
      this.artists = []
      this.loading = true

      getArtists(this.selectCountry)
      .then( (artists) => {
        for(let artist of artists){
          this.artists.push(artist)
        }
        this.loading = false
      }) 
    }
  },
  mounted() {
    this.refreshArtist()
  },
  watch: {
    selectCountry() {
      this.refreshArtist()
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

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
