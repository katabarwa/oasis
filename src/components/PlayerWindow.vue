<script setup>
import { ref } from 'vue'
// import  coverArt from '../assets/oasis_cover4.1.jpg'
import  coverArt2 from '../assets/oasis_cover3.4.jpg'

const count = ref(0)

// Import your audio files
import track1 from '../assets/audio/lostV4.m4a'
import track2 from '../assets/audio/The Way I do + _.m4a'
import track3 from '../assets/audio/sxetch.m4a'
import track4 from '../assets/audio/916.m4a'
import track5 from '../assets/audio/cruel_half_mix5.4.m4a'
import track6 from '../assets/audio/areyou.m4a'
import track7 from '../assets/audio/down4.m4a'
import track8 from '../assets/audio/oasis 101.m4a'
import track9 from '../assets/audio/thank you.m4a'


const tracks = [
{ title: '❧ you still', src: track9, disabled: true },
  { title: 'Never Mind', src: track1 },
  { title: 'The Way I Be + _', src: track2 },
  { title: 'In Flames', src: track3, disabled: true },
  { title: '916 interlude', src: track4, disabled: true },
  { title: 'Cruel Half', src: track5, disabled: true },
  { title: 'Are you down!?', src: track6, disabled: true },
  { title: 'f4ur', src: track7, disabled: true },
  { title: 'Oasis 101', src: track8, disabled: true },

]

const currentTrack = ref(null)
const audio = ref(null)

function play(track) {
  if (track.disabled) return
  if (audio.value) {
    audio.value.pause()
  }
  if (currentTrack.value?.title === track.title) {
    currentTrack.value = null
    return
  }
  currentTrack.value = track
  audio.value = new Audio(track.src)
  audio.value.play()
  audio.value.addEventListener('ended', playNext)
}

function playNext() {
  const currentIndex = tracks.findIndex(t => t.title === currentTrack.value?.title)
  const next = tracks.slice(currentIndex + 1).find(t => !t.disabled)
  if (next) play(next)
  else currentTrack.value = null
}
</script>

<template>
  <section id="center">
    <div class="hero">
      <img :src="coverArt2" class="base" />
    </div>
    <div>
      <h1>Oasis</h1>
      <p>by odyssey one</p>
    </div>
    <div class="tracklist">
      
      <h2
        v-for="track in tracks"
        :key="track.title"
       :class="{ active: currentTrack?.title === track.title, disabled: track.disabled }"
        @click="play(track)"
      
      >
        <span class="dot">•</span>{{ track.title }}
      </h2>
    
  </div>
    <!-- <button type="button" class="counter" @click="count++">
       {{ count }} listeners
    </button> -->

  </section>

  <!-- <div class="ticks"></div>

  <section id="next-steps">
    <div id="docs">
      <svg class="icon" role="presentation" aria-hidden="true">
        <use href="/icons.svg#documentation-icon"></use>
      </svg>
      <h2>Documentation</h2>
      <p>Your questions, answered</p>
      <ul>
        <li>
          <a href="https://vite.dev/" target="_blank">
            <img class="logo" :src="viteLogo" alt="" />
            Explore Vite
          </a>
        </li>
        <li>
          <a href="https://vuejs.org/" target="_blank">
            <img class="button-icon" :src="vueLogo" alt="" />
            Learn more
          </a>
        </li>
      </ul>
    </div>
    <div id="social">
      <svg class="icon" role="presentation" aria-hidden="true">
        <use href="/icons.svg#social-icon"></use>
      </svg>
      <h2>Connect with us</h2>
      <p>Join the Vite community</p>
      <ul>
        <li>
          <a href="https://github.com/vitejs/vite" target="_blank">
            <svg class="button-icon" role="presentation" aria-hidden="true">
              <use href="/icons.svg#github-icon"></use>
            </svg>
            GitHub
          </a>
        </li>
        <li>
          <a href="https://chat.vite.dev/" target="_blank">
            <svg class="button-icon" role="presentation" aria-hidden="true">
              <use href="/icons.svg#discord-icon"></use>
            </svg>
            Discord
          </a>
        </li>
        <li>
          <a href="https://x.com/vite_js" target="_blank">
            <svg class="button-icon" role="presentation" aria-hidden="true">
              <use href="/icons.svg#x-icon"></use>
            </svg>
            X.com
          </a>
        </li>
        <li>
          <a href="https://bsky.app/profile/vite.dev" target="_blank">
            <svg class="button-icon" role="presentation" aria-hidden="true">
              <use href="/icons.svg#bluesky-icon"></use>
            </svg>
            Bluesky
          </a>
        </li>
      </ul>
    </div>
  </section>

  <div class="ticks"></div>
  <section id="spacer"></section> -->
</template>

<style lang="css" scoped>
.tracklist {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100vw;
}

.tracklist h2 {
  display: flex;
  cursor: pointer;
}

.tracklist h2.disabled {
  opacity: 0.35;
  cursor: default;
  font-size:16px ;
}

.tracklist h2 .dot {
  display: none;
  margin-right: 6px;
}
.tracklist h2.active .dot {
  display: block;
  opacity: 1;
  font-size: 20px;
  color: orangered!important;
}
</style>
