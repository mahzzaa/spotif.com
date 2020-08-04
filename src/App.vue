<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{current.title}} ,
          <span>
            {{current.artist}}
          </span>
        </h2>
        <div class="control">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The PlayList</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{song.title}} , {{song.artist}}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
  export default{
    name:'app',
    data(){
      return{
        current:{
           },
           index:0,
           isPlaying: false,
           songs:[
             {
               title:'Grateful',
               artist:'Neffex',
               src: require('./assets/neffex-grateful.mp3')
             },
             {
               title:'Kourosh Yaghmaei',
               artist:' Havar Havar',
               src: require('./assets/Kourosh Yaghmaei - Havar Havar.mp3')
             }
           ],
           player: new Audio()
      }
    },
    methods:{
      play(song) {
        if (typeof song.src != "undefined"){
          this.current = song;
          this.player.src = this.current.src;
        }
         this.player.play();
         this.player.addEventListener('ended' ,function(){
           this.index++;
           if (this.index > this.songs.length - 1){
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
         }.bind(this));
         this.isPlaying = true;
      },
      pause(){
        this.player.pause();
        this.isPlaying = false;
      },
      next(){
        this.index++;
        if (this.index > this.songs.length - 1){
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      },
      prev(){
        this.index--;
        if (this.index < 0){
          this.index = this.songs.length - 1;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      }
    }
    ,
    created(){
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
      // this.player.play();
    }
  }
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Permanent Marker', cursive;

}

header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  /* background-color: rgba(127, 255, 212, 0.579); */
  background: linear-gradient(to right, #6ccfb6, #7c2a95bb);
  color: rgba(250, 235, 215, 0.815);
}

main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title{
  color: #5b756fc9;
  font-size: 20;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span{
  font-weight: 400;
  font-style: italic;
  padding: 30px 15px;
}

.control{
  display: flex;
  justify-content: center;
  padding: 30px 15px;
}

button{
  appearance: none;
  background: none;
  border: paleturquoise;
  outline: none;
  cursor: pointer;
}

.play , .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  color: #fff;
  background-color: #73e0c5c0;
  border-radius: 27px;
}

button:hover{
  opacity: 0.8;
}

.next, .prev{
  font-size: 16px;
  font-weight: 600;
  padding: 10px 20px;
  margin: 0px 15px;
  color: rgba(0, 0, 0, 0.452);
  background-color: #c7e9e0c0;
  border-radius: 100px;
}

.playlist{
  padding: 0px 30px;
}

.playlist h3{
  color: rgba(133, 180, 106, 0.849);
  font-size: 27px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song{
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
}

.playlist .song:hover{
  color:  rgb(97, 156, 70) ;
}

.playlist .song.playing{
  color: antiquewhite;
  border-radius: 25px;
  background-image:linear-gradient(to left , rgba(97, 156, 70, 0.794) , rgba(224, 214, 69, 0.835));
}
</style>