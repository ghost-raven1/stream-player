<template>
  <div class="home">
    <StreamPlayer :link="link" />
    <input v-model="link" />
  </div>
</template>

<script>

import StreamPlayer from '@/components/Player.vue'
export default {
  name: 'HomeView',
  components: {StreamPlayer},
  data() {
    return {
      link: 'https://live-streams.cdnvideo.ru/cdnvideo/caminandes/playlist.m3u8'
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 10px
}
</style>
