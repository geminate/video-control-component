<template>
  <div class="slider-bar" @click="onClick" ref="runWay">
    <div class="slider-bar-progress" :style="{height: progress + '%'}">
      <div class="slider-bar-button" @mousedown="onMouseDown"></div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SliderBar',
    props: ['value'],
    data () {
      return {
        progress: this.value
      }
    },
    watch: {
      value (newVal) {
        this.progress = newVal
      },
      progress (newVal) {
        this.$emit('input', newVal)
      }
    },
    methods: {
      onClick (e) {
        this.setProgress(e)
      },
      onMouseDown (e) {
        window.addEventListener('mousemove', this.onDragging)
        window.addEventListener('mouseup', this.onDragEnd)
      },
      onDragging (e) {
        this.setProgress(e)
      },
      onDragEnd (e) {
        this.setProgress(e)
        window.removeEventListener('mousemove', this.onDragging)
        window.removeEventListener('mouseup', this.onDragEnd)
      },
      setProgress (e) {
        const { height, top } = this.$refs.runWay.getBoundingClientRect()
        const percent = ((height - e.clientY + top) / height * 100).toFixed(0)
        this.progress = percent > 100 ? 100 : percent < 0 ? 0 : percent
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
