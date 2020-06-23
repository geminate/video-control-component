<!-- 视频预览 -->
<template>
  <div class="video-preview" :style="{left: previewX + 'px', backgroundPosition:backgroundPosition}"></div>
</template>

<script>
  export default {
    name: 'VideoPreview',
    props: ['previewX', 'previewPercent'], // 预览图片放置位置 , 预览进度百分比
    computed: {

      // 背景图片位置
      backgroundPosition () {
        const temp = Math.ceil(this.previewPercent / 100 / (1 / 48))
        const index = temp <= 1 ? 1 : temp >= 48 ? 48 : temp
        const imgX = index % 10 === 0 ? 9 : index % 10 - 1
        const imgY = index % 10 === 0 ? Math.floor(index / 10) - 1 : Math.floor(index / 10)
        return -imgX * 160 + 'px ' + -imgY * 90 + 'px'
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
