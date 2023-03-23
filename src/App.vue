<template>
  <div id="player" class="text-center shadow-lg rounded-lg overflow-hidden">
    <main class="rounded-lg overflow-hidden">
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="control flex items-center justify-center">
          <button class="prev" @click="prev"><ion-icon name="play-skip-back" class="text-3xl"></ion-icon></button>
          <button class="play rounded-full p-3 bg-grey-darker flex items-center justify-center" v-if="!isPlaying"
            @click="play"><ion-icon name="play" class="text-5xl"></ion-icon></button>
          <button class="pause rounded-full p-3 bg-grey-darker flex items-center justify-center" v-else
            @click="pause"><ion-icon name="pause-outline" class="text-5xl"></ion-icon></button>
          <button class="next" @click="next"><ion-icon name="play-skip-forward" class="text-3xl"></ion-icon></button>
        </div>
      </section>
      <section class="playlist border-t-2 border-grey-darkest">
        <h3 class="my-3">The Playlist</h3>
        <div class="flex flex-col text-white">
          <button v-for="song in songs" :key="song.src" @click="play(song)"
            class="p-2 bg-grey-dark border-t-2 border-grey-darkest py-3"
            :class="(song.src === current.src ? 'song playing' : 'song')">
            {{ song.title }} - {{ song.artist }}
          </button>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        { title: 'The Gentelman', artist: 'DivKid', src: require('@/assets/The Gentlemen - DivKid.mp3') },
        { title: 'Jingle Bells', artist: 'DJ Williams', src: require('@/assets/Jingle Bells - DJ Williams.mp3') },
      ],
      player: new Audio(),
    }
  },
  methods: {
    play(song) {
      if (typeof song.src !== "undefined") {
        this.current = song

        this.player.src = this.current.src
      }

      this.player.play()
      this.isPlaying = true
    },
    pause() {
      this.player.pause()
      this.isPlaying = false
    },
    next() {
      this.index++
      if (this.index > this.songs.length - 1) {
        this.index = 0
      }

      this.current = this.songs[this.index];
      this.play(this.current)
    },
    prev() {
      this.index--
      if (this.index < 0) {
        this.index = this.songs.length - 1
      }

      this.current = this.songs[this.index];
      this.play(this.current)
    },
  },
  created() {
    this.current = this.songs[this.index]
    this.player.src = this.current.src
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  display: grid;
  place-items: center;
  overflow: hidden;
  padding: 0 10px;
}

#player {
  max-width: 510px;
  width: 100%;
  padding: 20px 0 0 0;
  margin: 0 auto;
}

#player main {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
