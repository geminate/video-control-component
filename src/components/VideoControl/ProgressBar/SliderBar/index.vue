<template>
  <div class="slider-bar" @click="onClick" ref="runWay">
    <div class="buffered-bar" :style="{width: buffered + '%'}"></div>
    <div class="slider-bar-progress" :style="{width: progressData + '%'}">
      <div class="slider-bar-button" @mousedown="onMouseDown"></div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SliderBar',
    props: ['progress', 'buffered'],
    data () {
      return {
        progressData: this.progress,
        dragging: false
      }
    },
    watch: {
      progress () {
        !this.dragging && (this.progressData = this.progress)
      }
    },
    methods: {
      onClick (e) {
        this.setProgress(e)
        this.$emit('change', this.progressData)
      },
      onMouseDown (e) {
        this.onDragStart(e)
        window.addEventListener('mousemove', this.onDragging)
        window.addEventListener('mouseup', this.onDragEnd)
      },
      onDragStart (e) {
        this.dragging = true
      },
      onDragging (e) {
        this.setProgress(e)
      },
      onDragEnd (e) {
        this.setProgress(e)
        this.dragging = false
        this.$emit('change', this.progressData)
        window.removeEventListener('mousemove', this.onDragging)
        window.removeEventListener('mouseup', this.onDragEnd)
      },
      setProgress (e) {
        this.progressData = (e.clientX - this.$refs.runWay.getBoundingClientRect().left) / this.$refs.runWay.getBoundingClientRect().width * 100
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
