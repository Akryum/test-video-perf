<script>
import Animation from './Animation.vue'

export default {
  components: {
    Animation
  },

  props: {
    title: {
      type: String,
      required: true
    },

    overlayShown: {
      type: Boolean,
      default: false
    },

    animationShown: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<template>
  <div class="border border-gray-200 rounded relative">
    <div class="absolute inset-0">
      <slot />
    </div>

    <div v-if="overlayShown" class="absolute inset-0 bg-gradient-to-b from-black to-transparent text-white p-4 rounded">
      <div>{{ title }}</div>

      <div class="absolute inset-0 flex items-center justify-center">
        <img src="https://steamcdn-a.akamaihd.net/steamcommunity/public/images/avatars/a7/a7752bb87fccfec2a4bfe4d342937ebbadbfbd98_full.jpg" class="w-12 h-12 rounded-full">
      </div>

      <div class="absolute inset-0 flex items-center justify-center">
        <Animation v-if="animationShown" />
      </div>
    </div>
  </div>
</template>
