<template>
  <el-dropdown ref="speedDrop" class="speed-btn" @command="changeSpeed" placement="top">
    <span class="text">{{playSpeed === 1 ? '倍速' : playSpeed + 'x'}}</span>
    <el-dropdown-menu class="speed-dropdown-menu" slot="dropdown" :append-to-body="false">
      <el-dropdown-item command="0.5" :class="{active:playSpeed === 0.5}">0.5x</el-dropdown-item>
      <el-dropdown-item command="0.75" :class="{active:playSpeed === 0.75}">0.75x</el-dropdown-item>
      <el-dropdown-item command="1" :class="{active:playSpeed === 1}">1.0x</el-dropdown-item>
      <el-dropdown-item command="1.5" :class="{active:playSpeed === 1.5}">1.5x</el-dropdown-item>
      <el-dropdown-item command="2" :class="{active:playSpeed === 2}">2.0x</el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
  export default {
    name: 'VideoCore',
    props: ['value'],
    data () {
      return {
        playSpeed: this.value
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
        this.playSpeed = Number(playSpeed)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
