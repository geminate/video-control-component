<template>
  <el-dropdown ref="volumeDrop" class="volume-button" placement="top" :hide-on-click="false">
    <div class="volume-button-icon" :class="{'mute': volume === 0}" @click="toggleMute"></div>
    <el-dropdown-menu class="volume-dropdown-menu" slot="dropdown" :append-to-body="false">
      <el-dropdown-item>
        <p>{{volume}}%</p>
        <el-slider v-model="volume" vertical :show-tooltip="false"></el-slider>
      </el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
  export default {
    name: 'VideoCore',
    props: ['value'],
    data () {
      return {
        volume: this.value,
        showSlide: false
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
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
