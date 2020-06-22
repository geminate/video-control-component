<template>
  <div class="speed-btn" @mouseenter="onMouseEnter" @mouseleave="onMouseLeave">
    <span class="text">{{playSpeed === 1 ? '倍速' : playSpeed + 'x'}}</span>
    <div class="speed-dropdown-menu" :class="{'show':showDropDown}">
      <div v-for="item in speedArray"
           :key="item"
           class="speed-dropdown-item"
           :class="{active:playSpeed === item}"
           @click="changeSpeed(item)"
      >
        {{item}}x
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'VideoCore',
    props: ['value'],
    data () {
      return {
        playSpeed: this.value, // 当前播放速度
        speedArray: [0.5, 0.75, 1, 1.5, 2], // 可选速度数组
        showDropDown: false, // 是否显示下拉菜单
        timer: null // 下拉菜单展示延迟计时器
      }
    },
    watch: {
      value (newVal) {
        this.playSpeed = newVal
      },
      playSpeed (newVal) {
        this.$emit('input', newVal)
      }
    },
    methods: {
      changeSpeed (playSpeed) {
        this.playSpeed = playSpeed
        this.showDropDown = false
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
