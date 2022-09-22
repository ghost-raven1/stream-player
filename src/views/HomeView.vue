<template>
  <div class="home">
    <div class="home__container container">
      <StreamPlayer :link="link" class="container__player" />
      <div class="container__stream stream">
        <input v-model="currentLink" class="stream__input" />
        <button class="stream__button" @click="link = currentLink">Change stream</button>
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
      currentLink: 'https://live-streams.cdnvideo.ru/cdnvideo/caminandes/playlist.m3u8',
      link: 'https://live-streams.cdnvideo.ru/cdnvideo/caminandes/playlist.m3u8'
    }
  }
}
</script>

<style lang="scss" scoped>
.stream {
  display: grid;
  grid-template-columns: 8fr 3fr;
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
