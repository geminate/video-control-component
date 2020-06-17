<template>
  <div class="full-screen-btn" :class="{'is-full':isFullScreen}" @click="onClick"></div>
</template>

<script>
  export default {
    name: 'FullScreenBtn',
    props: ['value'],
    data () {
      return {
        isFullScreen: this.value
      }
    },
    watch: {
      value (newVal) {
        this.isFullScreen = newVal
      },
      isFullScreen (newVal) {
        this.$emit('input', newVal)
      }
    },
    methods: {
      onClick () {
        this.isFullScreen = !this.isFullScreen
      },

      // 初始化全屏
      initFullScreen () {
        document.addEventListener('fullscreenchange', this.onFullScreenChange)
        document.addEventListener('webkitfullscreenchange', this.onFullScreenChange)
        document.addEventListener('mozfullscreenchange', this.onFullScreenChange)
        document.addEventListener('MSFullscreenChange', this.onFullScreenChange)
      },

      // 切换全屏
      toggleFullScreen (element) {
        if (!this.isFullScreen) {
          this.exitFullscreen()
        } else {
          this.launchFullscreen(element)
        }
      },

      // 进入全屏
      launchFullscreen (element) {
        if (element.requestFullscreen) {
          element.requestFullscreen()
        } else if (element.mozRequestFullScreen) {
          element.mozRequestFullScreen()
        } else if (element.msRequestFullscreen) {
          element.msRequestFullscreen()
        } else if (element.webkitRequestFullscreen) {
          element.webkitRequestFullScreen()
        }
      },

      // 进入/退出全屏
      onFullScreenChange (e) {
        this.isFullScreen = !!document.fullscreenElement
      },

      // 退出全屏
      exitFullscreen () {
        if (document.exitFullscreen) {
          document.exitFullscreen()
        } else if (document.msExitFullscreen) {
          document.msExitFullscreen()
        } else if (document.mozCancelFullScreen) {
          document.mozCancelFullScreen()
        } else if (document.webkitExitFullscreen) {
          document.webkitExitFullscreen()
        }
      }
    },
    mounted () {
      this.initFullScreen()
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
