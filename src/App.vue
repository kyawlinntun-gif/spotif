<template>
    <div id="app">
        <header>
            <h1>My Music</h1>
        </header>
        <main>
            <section class="player">
                <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
            </section>
        </main>
        <div class="controls">
            <button class="prev" @click="prev">Prev</button>
            <button class="play" @click="play" v-if="!isPlaying">Play</button>
            <button class="pause" @click="pause" v-else>Pause</button>
            <button class="next" @click="next">Next</button>
        </div>
        <div class="playlist">
            <h3>Playlist</h3>
            <button v-for="song in songs" v-bind:key="song.src" :class="song.title == current.title ? 'song playing' : 'song'" @click="play(song)">
                {{ song.title }} - {{ song.artist }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            current: {},
            index: 0,
            songs: [
                {
                    title: 'Grateful',
                    artist: 'Neffex',
                    src: require('./assets/neffex-grateful.mp3')
                },
                {
                    title: 'Invincible',
                    artist: 'Deaf Kev',
                    src: require('./assets/deaf-kev-invincible.mp3')
                },
                {
                    title: 'SomeBody To You',
                    artist: 'The Vamps',
                    src: require('./assets/the-vamps-somebody-to-you.mp3')
                }
            ],
            player: new Audio(),
            isPlaying: false,
            song: null
        }
    },
    created() {
        this.current = this.songs[this.index];
        this.player.src = this.current.src;
    },
    methods: {
        play(song) {
            if(typeof song.src != "undefined")
            {
                this.current = song;
                this.player.src = this.current.src;
                this.index = this.songs.indexOf(song);
            }
            this.player.play();
            this.player.addEventListener('ended', function() {
                this.index++;
                if(this.index > this.songs.length - 1)
                {
                    this.index = 0;
                }
                this.current = this.songs[this.index];
                this.play(this.current);
            }.bind(this));
            this.isPlaying = true;
        },
        pause() {
            this.player.pause();
            this.isPlaying = false;
        },
        prev() {
            this.index--;
            if(this.index < 0)
            {
                this.index = this.songs.length - 1;
            }
            this.current = this.songs[this.index];
            this.play(this.current);
        },
        next() {
            this.index++;
            if(this.index > this.songs.length - 1)
            {
                this.index = 0;
            }
            this.current = this.songs[this.index];
            this.play(this.current);
        }
    }
}
</script>

<style>
* {
    margin: 0px;
    padding: 0px;
    box-sizing: border-box;
}
body {
    font-family: sans-serif;
}
header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 15px;
    background-color: #212121;
    color: #fff;
}
main {
    width: 100%;
    max-width: 768px;
    margin: 0 auto;
    padding: 25px;
}
.song-title {
    color: #53565A;
    font-size: 32px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
}
.song-title span {
    font-weight: 400;
    font-style: italic;
}
.controls {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px 15px;
}
button {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
}
button:hover {
    opacity: 0.8;
}
.play, .pause {
    font-size: 20px;
    font-weight: 700;
    padding: 15px 25px;
    margin: 0px 15px;
    border-radius: 8px;
    color: #FFF;
    background-color: #CC2E5D;
}
.next, .prev {
    font-size: 16px;
    font-weight: 700;
    padding: 10px 20px;
    margin: 0px 15px;
    border-radius: 6px;
    color: #FFF;
    background-color: #FF5858;
}
.playlist {
    padding: 0px 30px
}
.playlist h3 {
    color: #212121;
    font-size: 28px;
    font-weight: 400;
    margin-bottom: 30px;
    text-align: center;
}
.playlist .song {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
}
.playlist .song:hover {
    color: #FF5858;
}
.playlist .song.playing {
    color: #FFF;
    background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>
