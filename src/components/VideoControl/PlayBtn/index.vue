<!-- 播放/暂停 按钮 -->
<template>
  <div class="play-btn" @click="onClick">
    <svg viewBox="0 0 22 22" fill="white">
      <path v-if="!playStatus"
            d="M17.982 9.275L8.06 3.27A2.013 2.013 0 005 4.994v12.011a2.017 2.017 0 003.06 1.725l9.922-6.005a2.017 2.017 0 000-3.45z"></path>
      <path v-else
            d="M7 3a2 2 0 00-2 2v12a2 2 0 104 0V5a2 2 0 00-2-2zM15 3a2 2 0 00-2 2v12a2 2 0 104 0V5a2 2 0 00-2-2z"></path>
    </svg>
  </div>
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
