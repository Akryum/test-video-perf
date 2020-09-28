<script>
import { computed, ref } from 'vue'
import OT from '@opentok/client'
import VideoWrapper from './VideoWrapper.vue'
import Publisher from './Publisher.vue'
import Subscriber from './Subscriber.vue'

export default {
  name: 'App',

  components: {
    VideoWrapper,
    Publisher,
    Subscriber
  },

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

    const webRtcEnabled = ref(false)
    const overlayShown = ref(false)
    const animationShown = ref(false)

    // WebRTC

    function errorHandler (err) {
      console.error(err)
      alert(err.message)
    }

    const streams = ref([])

    const session = OT.initSession(process.env.VUE_APP_API_KEY, process.env.VUE_APP_SESSION_ID)
    session.connect(process.env.VUE_APP_TOKEN, err => {
      if (err) {
        errorHandler(err)
      }
    })

    session.on('streamCreated', event => {
      streams.value.push(event.stream)
    })

    session.on('streamDestroyed', event => {
      const idx = streams.value.indexOf(event.stream)
      if (idx > -1) {
        streams.value.splice(idx, 1)
      }
    })

    // List

    const list = computed(() => webRtcEnabled.value ? [
      'publisher',
      ...streams.value
    ] : videos.value)

    return {
      list,
      webRtcEnabled,
      overlayShown,
      animationShown,
      errorHandler,
      session
    }
  }
}
</script>

<template>
  <div class="w-screen h-screen flex flex-col">
    <div class="flex-none p-4 flex space-x-6 items-center">
      <label>
        <input type="checkbox" v-model="webRtcEnabled">
        Use WebRTC
      </label>

      <label>
        <input type="checkbox" v-model="overlayShown">
        Show overlay on videos
      </label>

      <label>
        <input type="checkbox" v-model="animationShown">
        Show animations in overlays
      </label>
    </div>
    <div class="flex-1 grid grid-cols-3 gap-4 p-4">
      <VideoWrapper
        v-for="item of list"
        :key="item"
        :title="item.id || item"
        :overlayShown="overlayShown"
        :animationShown="animationShown"
      >
        <template v-if="webRtcEnabled">
          <Publisher
            v-if="item === 'publisher'"
            :session="session"
            :opts="{
              mirror: true
            }"
          />
          <Subscriber
            v-else
            :stream="item"
            :session="session"
          />
        </template>
        <video v-else autoplay muted loop class="w-full h-full">
          <source :src="item" />
        </video>
      </VideoWrapper>
    </div>
  </div>
</template>

<style lang="postcss" scoped>
.grid {
  align-content: stretch;
}
</style>
