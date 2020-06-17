<template>
  <div class="progress-container" @click="onProgressClick">
    <el-slider v-model="timeProcess" :step="0.00001" :show-tooltip="false" ref="slider"></el-slider>
  </div>
</template>

<script>
  export default {
    name: 'VideoCore',
    props: ['value'],
    data () {
      return {
        timeProcess: 0
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
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
