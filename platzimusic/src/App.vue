<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 PlatziMusic
    select(v-model="selectdCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
    Loader(v-show="loading")  
    ul
      artist(v-for="artist in artists" :artist = "artist" :key="artist.mbid")
    
    
</template>

<script>
import getArtist from './api'
import Artist from './components/artists.vue'
import Loader from './components/Loader.vue'

//import Spinner from '/components/spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries : [
        {name : 'Ecuador' , value : 'ecuador'},
        {name : 'Colombia' , value : 'colombia'},
        {name : 'España' , value : 'spain'},

      ],
      selectdCountry : 'ecuador',
      loading : true
    }
  },
  components : {
    Artist : Artist,
    Loader : Loader,
  },

  methods : {
    refrestArtist() {
      const self = this
      self.loading = true
      getArtist(this.selectdCountry)
      .then(function (artists)  {
        self.loading = false
        self.artists = artists
      })
    }
  },
  mounted: function() {
    this.refrestArtist()
  },

  watch : {
    selectdCountry : function () {
      this.refrestArtist()
    }
  }
}
</script>

<style lang ="stylus">
  #app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px

h1, h2
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
