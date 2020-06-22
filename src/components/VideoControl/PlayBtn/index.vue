<!-- 播放/暂停 按钮 -->
<template>
  <div class="play-btn" @click="onClick" :class="{'pause': playStatus}"></div>
</template>

<script>
  export default {
    name: 'PlayBtn',
    props: ['value'],
    data () {
      return {
        playStatus: this.value
      }
    },
    watch: {
      value (newVal) {
        this.playStatus = newVal
      },
      playStatus (newVal) {
        this.$emit('input', newVal)
      }
    },
    methods: {
      onClick () {
        this.playStatus = !this.playStatus
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
