<template>
  <div></div>
</template>

<script>
import OT from '@opentok/client'

export default {
  name: 'subscriber',

  props: {
    stream: {
      type: OT.Stream,
      required: true
    },

    session: {
      type: OT.Session,
      required: true
    },

    opts: {
      type: Object,
      required: false
    }
  },

  mounted () {
    const subscriber = this.session.subscribe(
      this.stream,
      this.$el,
      this.opts,
      err => {
        if (err) {
          this.$emit('error', err)
        } else {
          this.$emit('subscriber-connected', subscriber)
        }
      }
    )
    this.$emit('subscriber-created', subscriber)
  }
}
</script>

<style lang="postcss" scoped>
div {
  @apply w-full h-full !important;
}
</style>
