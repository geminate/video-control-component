<template>
  <div class="volume-button" @mouseenter="onMouseEnter" @mouseleave="onMouseLeave">
    <div class="volume-button-icon" :class="{'mute': volume === 0}" @click="toggleMute"></div>
    <div class="volume-dropdown-menu" :class="{'show':showDropDown}">
      <div class="volume-dropdown-item">
        <p>{{volume}}%</p>
        <slider-bar class="slider-bar" v-model="volume"></slider-bar>
      </div>
    </div>
  </div>
</template>

<script>
  import SliderBar from './SliderBar'

  export default {
    name: 'VideoCore',
    components: { SliderBar },
    props: ['value'],
    data () {
      return {
        volume: this.value,
        showDropDown: false,
        timer: null // 下拉菜单展示延迟计时器
      }
    },
    watch: {
      value (newVal) {
        this.volume = newVal
      },
      volume (newVal) {
        this.$emit('input', newVal)
      }
    },
    methods: {
      // 静音切换
      toggleMute () {
        this.volume === 0 ? this.volume = 50 : this.volume = 0
      },
      onMouseEnter () {
        this.timer && clearTimeout(this.timer)
        this.timer = setTimeout(() => {
          this.showDropDown = true
        }, 250)
      },
      onMouseLeave () {
        this.timer && clearTimeout(this.timer)
        this.timer = setTimeout(() => {
          this.showDropDown = false
        }, 250)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
