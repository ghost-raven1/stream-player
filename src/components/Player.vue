<template>
  <div class="player" ref="player">
    <video class="player__video" ref="video" preload />
    <div class="player-container">
      <div class="player__progress progress">
        <div class="progress__video-position-time">{{ Math.ceil(videoDuration) }} sec</div>
        <progress class="progress__video-position" max="100" value="0" ref="position" />
        <input type="range" ref="volume" min="0" max="100" :value="progressValue" class="player__volume"
               @input="(v) => setVolume(v.target.value)" @click="(e) => videoRewind(e)">
      </div>
      <div class="player__buttons buttons">
        <button v-show="!isPlaying" @click="play" class="buttons__button">Play</button>
        <button v-show="isPlaying" @click="pause" class="buttons__button">Pause</button>
        <button @click="speedMinus" class="buttons__button">Speed-</button>
        <button @click="speedNormal" class="buttons__button">Normal</button>
        <button @click="speedPlus" class="buttons__button">Speed+</button>
      </div>
    </div>
  </div>
</template>

<script>
import Hls from 'hls.js';
export default {
  name: "StreamPlayer",
  data () {
    return {
      isPlaying: false,
      videoDuration: 0,
      buffer: 0,
      progressValue: 0,
      videoLink: 'https://live-streams.cdnvideo.ru/cdnvideo/caminandes/playlist.m3u8'
    }
  },
  computed: {
    player () {
      return this.$refs['player']
    },
    video () {
      return this.$refs['video']
    },
    volume () {
      return this.$refs['volume']
    },
    position () {
      return this.$refs['position']
    },
  },
  watch: {
    videoDuration () {
      let c = this.video.currentTime;
      let d = this.video.duration
      this.position.value = (100 * c) / d
    }
  },
  mounted() {
    let hls = new Hls();
    let stream = this.videoLink;
    const video = this.video
    hls.loadSource(stream);
    hls.attachMedia(video);
    hls.on(Hls.Events.LEVEL_LOADED, () => {
      this.videoDuration = hls.media.duration
    })
  },
  methods: {
    play () {
      this.isPlaying = true
      this.video.play()
    },
    pause () {
      this.isPlaying = false
      this.video.pause()
    },
    speedPlus () {
      this.video.playbackRate += 1
    },
    speedMinus () {
      if (this.video.playbackRate > 1) this.video.playbackRate -= 1
      else this.video.playbackRate = 1
    },
    speedNormal () {
      this.video.playbackRate = 1
    },
    setVolume (v) {
      this.video.volume = v / 100
    },
    videoRewind (e) {
      let w = this.offsetWidth
      let o = e.offsetX
      this.progressValue = 100 * (o/w)
      this.pause()
      this.video.currentTime = this.video.duration * (o/w)
    }
  }
}
</script>

<style lang="scss" scoped>
.player {
  display: flex;
  width: fit-content;
  flex-direction: column;
  justify-self: center;
  position: relative;
  &__video {
    width: 100%;
    min-width: 480px;
    border-radius: 13px;
  }
  &__buttons {
    position: absolute;
    display: flex;
    flex-direction: row;
    gap: 15px;
    bottom: 5%;
    left: 2%;
  }
  .player__volume {
    position: absolute;
    bottom: 16.5%;
    right: 2%;
  }
}
.progress {
  display: flex;
  flex-direction: row;
  justify-content: center;
  &__video-position {
    position: absolute;
    bottom: 17%;
    width: calc(100% - 350px);
  }
  &__video-position-time {
    width: fit-content;
    background: white;
    border-radius: 13px;
    position: absolute;
    bottom: 16%;
    left: 50px;
    padding: 5px 10px;
  }
}
.buttons__button {
  background: #3F888F;
  border-radius: 50px;
  padding: 15px;
  border: none;
  transition: .2s;
  box-shadow: 0 5px 10px 2px rgba(34, 60, 80, 0.2);
  font-size: 16px;
  color: white;
  &:hover {
    cursor: pointer;
    background: #5cbcc4;
  }
}
@include _1099 {
  .player__buttons {
    bottom: 1%;
    font-size: 14px;
    padding: 13px;
  }
}
@include _767 {
  .player__buttons {
    bottom: 0;
  }
  .buttons__button {
    font-size: 14px;
    padding: 5px;
  }
}
</style>
