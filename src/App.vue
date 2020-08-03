<template>
  <div id="app">
    <main class="main">
      <section class="player">
        <h1 class="app-title">Gilbert's Top {{songs.length}} Songs Playlist</h1>
        <h2 class="song-title">
          <span class="song-title__currently-playing">Currently Playing</span>
          <span class="song-title__song-artist">
            <span class="song-title__song-artist--title">{{current.title}}</span> by
            <span class="song-title__song-artist--artist">{{current.artist}}</span>
          </span>
        </h2>
        <div class="controls">
          <button v-on:click="prev" class="btn-control prev">Prev</button>
          <button v-on:click="play" v-if="!isPlaying" class="btn-control play">Play</button>
          <button v-on:click="pause" v-else class="btn-control pause">Pause</button>
          <button v-on:click="next" class="btn-control next">Next</button>
        </div>
        <div class="playlist">
          <h3 class="playlist__title">Click buttons below to select song</h3>
          <div class="playlist__buttons">
            <button v-for="(currentSong, index) in songs" v-bind:key="index" v-on:click="play(currentSong)" v-bind:class="(currentSong.src == current.src) ? 'song playing' : 'song'">{{currentSong.title}}</button>
          </div>
        </div>
      </section>
      <section class="album-artwork-display">
        <img v-bind:src="current.albumCover" alt="okocha" />
      </section>
    </main>
    <footer class="footer">
      <strong>
        <em>Disclaimer:</em>
      </strong> I
      <strong>do not</strong> own any of this music. All credit goes to the artists. In no way should this application be used to make any monetary gains. It is an application to display my Vue skills.
    </footer>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Enchant",
          artist: "Ibrahim",
          src: require("./assets/enchant.mp3"),
          albumCover: require("./assets/ibrahim.jpg")
        },
        {
          title: "Everyday",
          artist: "Unknown",
          src: require("./assets/everyday-gunna-youngthug.mp3"),
          albumCover: require("./assets/everyday.jpg")
        },
        {
          title: "Foggy",
          artist: "Wun Two",
          src: require("./assets/foggy.mp3"),
          albumCover: require("./assets/wuntwo.jpg")
        },
        {
          title: "home tomorrow",
          artist: "jhfly",
          src: require("./assets/home-tomorrow.mp3"),
          albumCover: require("./assets/jhfly.jpg")
        },
        {
          title: "The American Dream",
          artist: "18 Carat Affair",
          src: require("./assets/the-american-dream.mp3"),
          albumCover: require("./assets/18carataffair.jpg")
        }
      ],
      player: new Audio()
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function() {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    // this.player.play();
    // this.player.autoplay = true;
    // this.player.muted = false;
  }
};
</script>

<style lang="scss">
@import "./App.css";
button {
  background-color: #fff;
  &:focus {
    outline: none;
  }
}

.app-title {
  font-size: 3rem;
  text-align: center;
  text-transform: uppercase;
  text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 15px #fff, 0 0 20px #2196f3,
    0 0 30px #2196f3, 0 0 40px #2196f3, 0 0 55px #2196f3, 0 0 75px #2196f3,
    2px 2px 2px rgba(13, 84, 206, 0);
  letter-spacing: 2.5px;
}

.song-title {
  text-align: center;

  & > * {
    display: block;
  }

  &__currently-playing {
    font-size: 2rem;
    margin-bottom: 1.5rem;
  }

  &__song-artist {
    &--title {
      text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 15px #fff, 0 0 20px #fff,
        0 0 30px #fff, 0 0 40px #fff, 0 0 55px #fff, 0 0 75px #fff,
        2px 2px 2px rgba(13, 84, 206, 0);
    }
  }
}

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1.5rem;
  background-color: #212121;
  color: #fff;
}

.main {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  height: 90%;

  .player {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
  }

  .controls {
    display: flex;
    justify-content: space-between;
    margin: 0 5rem;

    .btn-control {
      display: inline-block;
      padding: 1.2rem 2.5rem;
      border-radius: 5px;
      color: #2196f3;
      font-size: 2.2rem;
      cursor: pointer;
      transition: all 0.2s ease;
      border: none;

      &:hover {
        color: #fff;
        background-color: #2196f3;
        box-shadow: 0 0 1rem #2196f3, 0 0 2rem #2196f3, 0 0 4rem #2196f3;
      }
    }
  }

  .playlist {
    &__title {
      text-align: center;
      margin-bottom: 1rem;
      font-size: 1.6rem;
    }
    &__buttons {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(3rem, 1fr));
      column-gap: 1.4rem;
      margin: 0 2.5rem;
      & button {
        border: none;
        border-radius: 10px;
        color: #2196f3;
        cursor: pointer;
        padding: 0.5rem 1rem;
        &:hover {
          color: #fff;
          background-color: #2196f3;
          box-shadow: 0 0 1rem #2196f3, 0 0 2rem #2196f3, 0 0 4rem #2196f3;
        }
      }
    }
  }

  .album-artwork-display {
    & img {
      object-fit: cover;
      width: 100%;
      height: 100%;
      max-height: 100%;
    }
  }
}

.footer {
  text-align: center;
  margin-top: 2rem;
  font-size: 1.3rem;
}
</style>
