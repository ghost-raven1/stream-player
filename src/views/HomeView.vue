<template>
  <div class="home">
    <div class="home__container container">
      <StreamPlayer :link="link" :isVideo="isVideo" class="container__player" />
      <select class="container__select select" v-if="!isVideo" v-model="currentLink">
        <option class="select__item" v-for="(item, i) in radioList" :key="i" :disabled="item.disabled"
                :value="item.link">{{ item.title }}</option>
      </select>
      <div class="container__stream stream">
        <input v-model="currentLink" class="stream__input" />
        <button class="stream__button" @click="link = currentLink">Change stream</button>
        <button class="stream__button" @click="isVideo = !isVideo">{{ isVideo ? 'Video' : 'Audio' }}</button>
      </div>
    </div>
  </div>
</template>

<script>

import StreamPlayer from '@/components/Player.vue'
export default {
  name: 'HomeView',
  components: {StreamPlayer},
  data() {
    return {
      isVideo: true,
      currentLink: 'https://live-streams.cdnvideo.ru/cdnvideo/caminandes/playlist.m3u8',
      link: 'https://live-streams.cdnvideo.ru/cdnvideo/caminandes/playlist.m3u8',
      radioList: [
        {
          disabled: true,
          title: 'Please Select',
          link: ''
        },
        {
          disabled: false,
          title: 'Sputnik 256',
          link: 'https://audio2.video.ria.ru:80/voicerushi'
        },
        {
          disabled: false,
          title: 'Sputnik 128',
          link: 'https://audio2.video.ria.ru:80/voicerus'
        },
        {
          disabled: false,
          title: 'Маяк 192',
          link: 'https://icecast.vgtrk.cdnvideo.ru/mayakfm_mp3_192kbps'
        },
        {
          disabled: false,
          title: 'Радио России 192',
          link: 'https://icecast.vgtrk.cdnvideo.ru/rrzonam_mp3_192kbps'
        }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.select {
  height: 30px;
}
.stream {
  display: grid;
  grid-template-columns: 8fr 3fr 3fr;
  gap: 10px;
  width: 100%;
  &__button {
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
  &__input {
    font-size: 16px;
    padding-left: 10px;
    margin-right: 10px;
    border-radius: 16px;
  }
}
.home {
  display: flex;
  height: 100%;
  width: 100%;
  justify-content: center;
}
.container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  &__player {
    max-width: 1300px;
    width: 100%;
  }
  &__input {
    max-width: 1300px;
    width: 100%;
  }
}
</style>
