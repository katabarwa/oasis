<script setup>
import { ref, computed, onMounted } from 'vue'
import  coverArt from '../assets/oasis_cover4.1.jpg'
import  coverArt2 from '../assets/oasis_cover3.4.jpg'

import { track as trackEvent } from '@vercel/analytics'

const count = ref(0)

const oasis101Video = 'https://pub-ad073cb475024372b387245e17bdfd68.r2.dev/oasis_101_.mp4'
const videoEl = ref(null)
const needsTapForSound = ref(false)

// Import your audio files
import track1 from '../assets/audio/neverMind.m4a'
import track2 from '../assets/audio/The Way I do + _.m4a'
import track3 from '../assets/audio/sxetch.m4a'
import track4 from '../assets/audio/916.m4a'
import track5 from '../assets/audio/cruel_half_mix5.4.m4a'
import track6 from '../assets/audio/AREYOUALIVE_last.m4a'
import track7 from '../assets/audio/down4.m4a'
import track8 from '../assets/audio/oasis 101.m4a'
import track9 from '../assets/audio/thank you.m4a'
import track10 from '../assets/audio/FeelYou.m4a'
import track11 from '../assets/audio/onSilent.m4a'
import track12 from '../assets/audio/babii.m4a'
import track13 from '../assets/audio/SyrenMars.m4a'
import track14 from '../assets/audio/if_only.m4a'

const tracks = [

  { title: '❧ y0u still', src: track9, disabled: false },
  { title: '916 interlude', src: track4, disabled: false },
  { title: 'Are you alive!?', src: track6, disabled: false },
  { title: 'Feel You', src: track10, disabled: false },
  { title: 'The Way I Be + _', src: track2, disabled: false },
  { title: 'On Silent', src: track11, disabled: false },
  { title: 'In Flames', src: track3, disabled: false },
  { title: 'Cruel Half', src: track5, disabled: false },
  { title: 'Never Mind', src: track1, disabled: false },
  { title: 'f4r u', src: track7, disabled: false },
  { title: 'Syren Mars', src: track13, disabled: false },
  { title: 'bby', src: track12, disabled: false },
  { title: 'if only', src: track14, disabled: false },
  { title: 'Oasis 101', src: track8, disabled: false },



]

const currentTrack = ref(null)
const audio = ref(null)

const showVideo = computed(() => currentTrack.value?.title === 'Oasis 101')

onMounted(() => {
  videoEl.value?.load()
})

async function play(track) {
  if (track.disabled) return

  if (audio.value) {
    audio.value.pause()
  }
  if (videoEl.value && !videoEl.value.paused) {
    videoEl.value.pause()
    videoEl.value.currentTime = 0
  }
  needsTapForSound.value = false

  if (currentTrack.value?.title === track.title) {
    currentTrack.value = null
    return
  }

  currentTrack.value = track
  trackEvent('play_track', { trackName: track.title })  // ← analytics event

  audio.value = new Audio(track.src)
  audio.value.addEventListener('ended', playNext)

  if (track.title === 'Oasis 101' && videoEl.value) {
    audio.value.muted = true
    audio.value.play().catch(() => {})

    videoEl.value.currentTime = 0
    videoEl.value.muted = false

    try {
      await videoEl.value.play()
    } catch (err) {
      // Safari blocked unmuted autoplay - play muted and ask for a tap to unmute
      videoEl.value.muted = true
      videoEl.value.play().catch(() => {})
      needsTapForSound.value = true
    }
  } else {
    audio.value.muted = false
    audio.value.play().catch(() => {})
  }
}

function enableSound() {
  if (videoEl.value) {
    videoEl.value.muted = false
  }
  needsTapForSound.value = false
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
      <img v-show="!showVideo" :src="coverArt2" class="base" />
      <video
        ref="videoEl"
        v-show="showVideo"
        :src="oasis101Video"
        class="base"
        preload="auto"
        playsinline
      />
      <button
        v-if="showVideo && needsTapForSound"
        class="sound-btn"
        @click="enableSound"
      >
        🔇 tap for sound
      </button>
    </div>
    <audio ref="audioEl" @ended="playNext" playsinline />
    <div>
      <h1>Oasis</h1>
      <p id="credit">by Odyssey One</p>
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
  </section>
</template>

<style lang="css" scoped>
#credit {
  letter-spacing: -1px;
}

.hero {
  position: relative;
}

.sound-btn {
  position: absolute;
  bottom: 12px;
  right: 12px;
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  border-radius: 20px;
  padding: 6px 14px;
  font-size: 13px;
  cursor: pointer;
}

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
  font-size: 16px;
}

.tracklist h2 .dot {
  display: none;
  margin-right: 6px;
}
.tracklist h2.active .dot {
  display: block;
  opacity: 1;
  font-size: 20px;
  color: orangered !important;
}
</style>