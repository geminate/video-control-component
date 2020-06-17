<template>
  <el-dropdown ref="volumeDrop" class="volume-button" :class="{mute:volume === 0}" trigger="click" placement="top"
               :hide-on-click="false">
    <span></span>
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
    methods: {}
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
