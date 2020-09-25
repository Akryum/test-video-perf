<script>
import { ref } from 'vue'
import Animation from './Animation.vue'

export default {
  name: 'App',

  components: {
    Animation
  },

  setup () {
    const videos = ref([
      'https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm',
      'http://techslides.com/demos/sample-videos/small.mp4',
      'http://clips.vorwaerts-gmbh.de/VfE_html5.mp4',
      'https://vjs.zencdn.net/v/oceans.mp4',
      'https://d2zihajmogu5jn.cloudfront.net/elephantsdream/ed_hd.mp4',
      'https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm',
      'https://www.w3schools.com/html/mov_bbb.mp4',
      'https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4',
      'https://media.w3.org/2010/05/sintel/trailer.mp4'
    ])

    const overlayShown = ref(false)

    return {
      videos,
      overlayShown
    }
  }
}
</script>

<template>
  <div class="w-screen h-screen flex flex-col">
    <div class="flex-none p-4">
      <label>
        <input type="checkbox" v-model="overlayShown">
        Show overlay on videos
      </label>
    </div>
    <div class="flex-1 grid grid-cols-3 gap-4 p-4">
      <div
        v-for="video of videos"
        :key="video"
        class="border border-gray-200 rounded relative"
      >
        <div class="absolute inset-0">
          <video autoplay muted loop class="w-full h-full">
            <source :src="video" />
          </video>
        </div>

        <div v-if="overlayShown" class="absolute inset-0 bg-gradient-to-b from-black to-transparent text-white p-4 rounded">
          <div>{{ video }}</div>

          <div class="absolute inset-0 flex items-center justify-center">
            <img src="https://steamcdn-a.akamaihd.net/steamcommunity/public/images/avatars/a7/a7752bb87fccfec2a4bfe4d342937ebbadbfbd98_full.jpg" class="w-12 h-12 rounded-full">
          </div>

          <div class="absolute inset-0 flex items-center justify-center">
            <Animation />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="postcss" scoped>
.grid {
  align-content: stretch;
}
</style>