<template>
  <div class="progress-container">

    <!-- 高能进度条 -->
    <focus-preview class="focus-preview"></focus-preview>

    <!-- 视频预览 -->
    <video-preview v-show="showPreview" class="video-preview" :previewX="previewX" :previewPercent="previewPercent">
    </video-preview>

    <!-- 进度条 -->
    <div ref="progressContainer"
         @mouseenter="onMouseEnter"
         @mouseleave="onMouseLeave"
         @mousemove="onMouseMove">
      <slider-bar class="slider-bar" :progress="progress" @change="onChange"></slider-bar>
    </div>
  </div>
</template>

<script>
  import VideoPreview from './VideoPreview'
  import FocusPreview from './FocusPreview'
  import SliderBar from './SliderBar'

  export default {
    name: 'VideoCore',
    components: { VideoPreview, FocusPreview, SliderBar },
    props: ['progress'],
    data () {
      return {
        showPreview: false,
        previewX: 0,
        previewPercent: 0
      }
    },
    methods: {
      onMouseEnter () {
        this.showPreview = true
      },
      onMouseLeave () {
        this.showPreview = false
      },
      onMouseMove (e) {
        let temp = e.clientX - this.$refs.progressContainer.getBoundingClientRect().x
        this.previewPercent = temp / this.$refs.progressContainer.getBoundingClientRect().width
        if (temp <= 80) {
          temp = 80
        } else if (temp > this.$refs.progressContainer.getBoundingClientRect().width - 80) {
          temp = this.$refs.progressContainer.getBoundingClientRect().width - 80
        }
        this.previewX = temp
      },
      onChange (val) {
        this.$emit('change', val)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
