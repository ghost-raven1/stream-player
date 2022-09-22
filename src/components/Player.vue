<template>
  <div class="player" ref="player">
    <video v-show="!isError" class="player__video" ref="video" />
    <div v-show="!isError" class="player-container">
      <div class="player__progress progress">
        <div class="progress__video-position-time">{{ videoDurationView }} {{ videoDurationTotal }}</div>
        <progress class="progress__video-position" value="0" max="100" ref="position" @click="(e) => videoRewind(e)" />
        <input type="range" ref="volume" min="0" max="100" value="100" class="player__volume"
               @input="(v) => setVolume(v.target.value)">
      </div>
      <div v-show="!isError" class="player__buttons buttons">
        <button v-show="!isPlaying" @click="play" class="buttons__button">Play</button>
        <button v-show="isPlaying" @click="pause" class="buttons__button">Pause</button>
        <button @click="speedMinus" class="buttons__button">Speed-</button>
        <button @click="speedNormal" class="buttons__button">Normal</button>
        <button @click="speedPlus" class="buttons__button">Speed+</button>
      </div>
    </div>
    <div v-show="isError" class="player__video">
      Error in stream link
    </div>
  </div>
</template>

<script>
import Hls from 'hls.js';
export default {
  name: "StreamPlayer",
  props: {
    link: {
      type: String
    }
  },
  data () {
    return {
      isPlaying: false,
      isError: false,
      videoDurationTotal: '0: 00',
      videoDurationView: '0: 00',
      buffer: 0,
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
    link () {
      this.initPlayer()
    },
    videoDurationView () {
      let c = this.video.currentTime;
      let d = this.video.duration
      this.position.value = (100 * c) / d
    }
  },
  mounted() {
    this.initPlayer ()
  },
  methods: {
    calcVideoDuration (duration, type) {
      this.$data[type] = Math.floor(duration / 60) + ':' + Math.ceil(duration % 60)
    },
    videoRewind(e) {
      let w = this.position.offsetWidth
      let o = e.offsetX
      this.position.value = 100 * o/w
      this.pause()
      this.video.currentTime = this.video.duration * (o/w)
      this.play()
    },
    initPlayer () {
      this.isError = false
      let hls = new Hls({
        enableWorker: true
      });
      let stream = this.link;
      const video = this.video
      hls.loadSource(stream);
      hls.attachMedia(video);
      hls.on(Hls.Events.ERROR, () => {
        this.isError = true
        this.initPlayer()
      })
      hls.on(Hls.Events.LEVEL_LOADED, () => {
        this.calcVideoDuration(hls.levels[0].details.totalduration, 'videoDurationTotal')
        if (this.isPlaying) this.calcVideoDuration(hls.media.duration, 'videoDurationView')
      })
    },
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
    max-width: 1300px;
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
