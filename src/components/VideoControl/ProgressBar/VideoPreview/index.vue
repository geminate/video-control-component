<template>
  <div class="video-preview" :style="{left: previewX + 'px',backgroundPosition:backgroundPosition}"
       ref="videoPreview"></div>
</template>

<script>
  export default {
    name: 'VideoPreview',
    props: ['previewX', 'previewPercent'],
    data () {
      return {
        imgX: 0,
        imgY: 0
      }
    },
    computed: {
      backgroundPosition () {
        return -this.imgX * 160 + 'px ' + -this.imgY * 90 + 'px'
      }
    },
    watch: {
      previewPercent (val) {
        const temp = Math.ceil(val / (1 / 48))
        const index = temp <= 1 ? 1 : temp >= 48 ? 48 : temp
        this.imgX = index % 10 === 0 ? 9 : index % 10 - 1
        this.imgY = index % 10 === 0 ? Math.floor(index / 10) - 1 : Math.floor(index / 10)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
