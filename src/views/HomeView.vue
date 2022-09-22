<template>
  <div class="home">
    <div class="home__container container">
      <StreamPlayer :link="link" class="container__player" />
      <input v-model="currentLink" class="container__input" />
      <button @click="link = currentLink">Change stream</button>
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
