<template>
  <div class="progress-container" @click="onProgressClick" @mouseenter="onMouseEnter" @mouseleave="onMouseLeave"
       @mousemove="onMouseMove" ref="progressContainer">
    <video-preview v-show="showPreview" class="video-preview" :previewX="previewX"
                   :previewPercent="previewPercent"></video-preview>
    <el-slider v-model="timeProcess" :step="0.00001" :show-tooltip="false" ref="slider"></el-slider>
  </div>
</template>

<script>
  import VideoPreview from './VideoPreview'

  export default {
    name: 'VideoCore',
    components: { VideoPreview },
    props: ['value'],
    data () {
      return {
        timeProcess: 0,
        showPreview: false,
        previewX: 0,
        previewPercent: 0
      }
    },
    watch: {
      value (newVal) {
        this.timeProcess = newVal
      },
      timeProcess (newVal) {
        this.$emit('input', newVal)
      }
    },
    methods: {
      isDragging () {
        return this.$refs.slider.dragging
      },
      onProgressClick () {
        this.$emit('click')
      },
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
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
