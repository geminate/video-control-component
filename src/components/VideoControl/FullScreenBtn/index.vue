<template>
  <div class="full-screen-btn" @click="onClick">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 22" fill="white">
      <path
        d="M20 11h-6c0-1.2-.7-2.2-1.7-2.7L15 2.9c3 1.5 5 4.6 5 8.1zM15.8 18.6c-1.4.9-3 1.4-4.8 1.4s-3.4-.5-4.8-1.4l2.7-5.4c.6.5 1.3.8 2.1.8s1.5-.3 2.1-.8l2.7 5.4zM11 8V2v6zM7 2.9l2.7 5.4C8.7 8.8 8 9.8 8 11H2c0-3.5 2-6.6 5-8.1z"></path>
      <circle class="st0" cx="11" cy="11" r="1"></circle>
    </svg>
  </div>
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
