<script>
import { ref } from 'vue'

export default {
  name: 'App',

  setup () {
    const videos = ref([
      'https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm',
      'https://www.nicolas-hoffmann.net/animations/Cavernae_Terragen2.mp4',
      'http://github.com/mediaelement/mediaelement-files/blob/master/big_buck_bunny.mp4?raw=true',
      'https://vjs.zencdn.net/v/oceans.mp4',
      'https://d2zihajmogu5jn.cloudfront.net/elephantsdream/ed_hd.mp4',
      'https://storage.googleapis.com/webfundamentals-assets/videos/chrome.webm',
      'https://www.w3schools.com/html/mov_bbb.mp4',
      'https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4',
      'https://media.w3.org/2010/05/sintel/trailer.mp4'
    ])

    // Settings

    const br = ref(false)
    const differentBr = ref(false)

    return {
      videos,
      br,
      differentBr
    }
  }
}
</script>

<template>
  <div class="w-screen h-screen flex flex-col">
    <div class="flex-none p-4 flex space-x-6 items-center">
      <label>
        <input type="checkbox" v-model="br">
        Border radius
      </label>

      <label>
        <input type="checkbox" v-model="differentBr">
        Different border radius
      </label>
    </div>
    <div class="flex-1 grid grid-cols-3 gap-4 p-4">
      <div
        v-for="video of videos"
        :key="video"
        class="bg-gray-400 p-2 flex items-center justify-center"
      >
        <video autoplay muted loop class="min-w-full max-w-full max-h-full"
          :class="br ? !differentBr ? 'rounded-lg' : 'rounded-tl rounded-tr-lg rounded-bl-sm rounded-br-md' : null"
        >
          <source :src="video" />
        </video>
      </div>
    </div>
  </div>
</template>

<style lang="postcss" scoped>
.grid {
  align-content: stretch;
}
</style>
