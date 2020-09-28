<template>
  <div></div>
</template>

<script>
import OT from '@opentok/client'

export default {
  name: 'publisher',

  props: {
    session: {
      type: OT.Session,
      required: false
    },

    opts: {
      type: Object,
      required: false
    }
  },

  mounted () {
    const publisher = OT.initPublisher(this.$el, this.opts, err => {
      if (err) {
        this.$emit('error', err)
      } else {
        this.$emit('publisher-completed')
      }
    })
    this.$emit('publisher-created', publisher)
    const publish = () => {
      this.session.publish(publisher, err => {
        if (err) {
          this.$emit('error', err)
        } else {
          this.$emit('publisher-connected', publisher)
        }
      })
    }
    if (this.session && this.session.isConnected()) {
      publish()
    }
    if (this.session) {
      this.session.on('sessionConnected', publish)
    }
  }
}
</script>

<style lang="postcss" scoped>
div {
  @apply w-full h-full !important;
}
</style>
