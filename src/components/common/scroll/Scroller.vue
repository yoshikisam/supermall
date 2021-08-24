<template>
<div ref="wrapper">
  <div>
    <slot></slot>
  </div>
</div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: `Scroller`,
  data() {
    return {
      scroller: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  },
  props: {
      probeType: {
        type: Number,
        default: 0
      },
    pullUpLoad: {
        type: Boolean,
        default: false
    }
  },
  mounted() {
    this.scroller = new BScroll(this.$refs.wrapper, {
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad,
      click: true,
      mouseWheel: true,
      observeDOM: true

    })
    this.scroller.on('scroll', (position) => {
      this.$emit('scroll', position)
    })
    this.scroller.on('pullingUp', () => {
     this.$emit('pullingUp')
    })
  },
  methods: {
    scrollTo(x, y, time=300) {
      this.scroller.scrollTo(0, 0, time)
    },
    finishPullUp() {
      this.scroller.finishPullUp()
    }
  }
}
</script>

<style scoped>

</style>