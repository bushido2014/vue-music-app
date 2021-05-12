<template>
  <div id="app" class="antialiased font-sans">
    
    <header class="bg-blue-900 py-8 border-b border-gray-500">
      <div class="max-w-2xl mx-auto">
      <h1 class="font-bold text-white text-center text-4xl leading-9">My Music</h1>
      </div>
    </header>
    <main class="max-w-2xl mx-auto py-12">
      <section class="player">
        <h2 class="text-3xl uppercase font-bold text-center leading-2">{{current.title}} - <span class="italic font-semibold">{{current.artist}}</span></h2>
        <div class="flex justify-center items-center py-9 my-8">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3 class="font-normal text-3xl mb-8 text-center">The Playlist</h3>
        <div v-for="song in songs" :key="song.src" @click="play(song)"
         :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{song.title}} -- {{song.artist}}
        </div>
      </section>
    </main>
    </div>
 
</template>

<script>


export default {
 name: 'app',
 data() {
   return {
     current: {}, 
     index: 0,
     isPlaying: false,
     songs: [
       {
         title: 'Immensity',
         artist: 'Jella',
         src: require('./assets/jella-immensity.mp3')
       },
       {
         title: 'AK Discovery',
         artist: 'Neffix',
         src: require('./assets/ak-discovery.mp3')
       },
       {
         title: 'Rasi Zamin',
         artist: 'Rasi',
         src: require('./assets/rasi_zamin.mp3')
       }
     ], 
     player: new Audio()
   }
 },
 methods: {
    play(song) {
       if (typeof song.src != "undefined") {
         this.current = song

         this.player.src = this.current.src
       }
       this.player.play()
       this.player.addEventListener('ended', function(){
           this.index++
           if (this.index > this.songs.length -1) {
        this.index = 0
      }
     this.current = this.songs[this.index]
     this.play(this.current)

       }.bind(this))
       this.isPlaying = true
    },
    pause() {
      this.player.pause()
      this.isPlaying = false
    },
    next() {
      this.index++;
      if (this.index > this.songs.length -1) {
        this.index = 0
      }
     this.current = this.songs[this.index]
     this.play(this.current)
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length -1
      }
     this.current = this.songs[this.index]
     this.play(this.current)
    }
 },
 created () {
   this.current = this.songs[this.index]
   this.player.src = this.current.src
   //this.player.play()
 }
}
</script>




<style>
#app {
  min-width: 320px;
}



</style>
