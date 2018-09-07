<template>
    <div id="app">
      <div id="music-container">
        <span id="title" class="song-text">{{songTitle}}</span>
        <span id="artist" class="song-text">{{songArtist}}</span>
        <img class="playing-artwork" :src=artwork>
      </div>
    </div>
  </template>
  
<script>
    /* eslint-disable */
  import HelloWorld from './components/HelloWorld.vue'
  import Token from './token.json'
  const spotifyAPI = 'https://api.spotify.com/v1/me/player/currently-playing'
  const tokenAPI = 'http://10.125.32.133/token'

  let currentRequest = false

  export default {
    name: 'app',
    components: { HelloWorld },
    data() {
      return {
        songTitle: '',
        songArtist: '',
        duration: '',
        artwork: '',
        token: Token.access_token
      }
    },
    async created() {
      this.getCurrentSong()
      setInterval(() => { this.getCurrentSong() }, 1000)
    },
    methods: {
      async getCurrentSong () {
        const request = {
          method: 'GET', 
          headers: {
            'Accept': 'application/json, text/plain, */*',
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${this.token}`
          }
        }
  
        fetch(spotifyAPI, request)
          .then(res => {
            if(res.status === 401 && currentRequest) {
              currentRequest = True
              await this.refreshToken()
              currentRequest = False
              this.getCurrentSong()
              return ;
            }
          })
          .then(res => res.json())
          .then(res => {
            this.artwork = res.item.album.images[0].url
            this.songTitle = res.item.name
            this.songArtist = res.item.artists[0].name
          })
      },
      async refreshToken () {
        const request = {
          method: 'GET', 
          headers: {
            'Accept': 'application/json, text/plain, */*',
            'Content-Type': 'application/json',
          }
        }
          
        let res = (await fetch(tokenAPI, request).then(res => res.json()))
        this.token = res.access_token
      }
    }
  }
</script>
  
<style>
  @import url('https://fonts.googleapis.com/css?family=Abril+Fatface');
  
  * {
    color: #eee;
    font-family: 'Abril Fatface', cursive;
  }

  .song-text {
    display: block;
    font-weight: 200;
  }

  body {
    margin: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: white;
  }

  ::selection { background: yellow; }

  #app {
    text-align: center;
    width: 100%;
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
  }

  #phone {
    height: 90vh;
  }
  
  #current-text {
    max-width: 50%;
    font-size: 40px;
    font-style: italic;
    font-weight: 900;
  }
  
  #music-container {
    line-height: 18px;
    padding-top: 18px;
    background-color: black;
    height: 405px;
  }

  .playing-artwork {
    width: 355px;
  }

  #artist {
    font-size: 11px;
    color: rgba(255, 255, 255, 0.6)
  }
  
  .playing-artwork {
    margin-top: 15px;
  }
</style>
  