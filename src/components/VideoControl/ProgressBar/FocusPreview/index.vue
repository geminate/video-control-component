<!-- 高能进度条 -->
<template>
  <svg class="focus-preview" viewBox="0 0 1000 100" preserveAspectRatio="none">
    <defs>
      <!-- 曲线剪切 -->
      <clipPath id="curve-path">
        <path :d='getCurvePath()'></path>
      </clipPath>

      <!-- 已播放区域剪切 -->
      <clipPath id="played-path">
        <rect v-for="(item,i) in playedDuration.playedArray"
              :key="i"
              :x="item[0]+'%'"
              :width="item[1]+'%'"
              y="0"
              height="100%"></rect>
        <rect :x="playedDuration.playingDuration[0] + '%'"
              :width="playedDuration.playingDuration[1] + '%'"
              y="0"
              height="100%"></rect>
      </clipPath>
    </defs>
    <g fill-opacity="0.2" clip-path="url(#curve-path)">
      <rect x="0" y="0" width="100%" height="100%" fill="rgb(255,255,255)"></rect>
      <rect x="0" y="0" width="100%" height="100%" fill="rgb(35,173,229)" clip-path="url(#played-path)"></rect>
    </g>
  </svg>
</template>

<script>
  import { value } from '../../../../mock/focus-preview.js'

  export default {
    name: 'FocusPreview',
    props: ['playedDuration'], // 已播放进度数据
    data () {
      return {
        curveData: value // 高能进度条数据
      }
    },
    methods: {

      // 获取曲线剪切路径
      getCurvePath () {
        const min = Math.min(...this.curveData), max = Math.max(...this.curveData)
        const a = 80 / (min - max), b = 80 - (min * 80) / (min - max) // y = ax + b , 过 (min , 80) (max , 0) 两点，求 a , b
        let re = 'M 0 100 '
        for (let i = 0; i < 100; i++) {
          if (i === 0) {
            re += `L 0 ${b} `
          } else {
            re += `C ${5 * 2 * i - 5} ${a * this.curveData[(i - 1)] + b}, ${5 * 2 * i - 5} ${a * this.curveData[i] + b}, ${5 * 2 * i} ${a * this.curveData[i] + b} `
          }
        }
        re += 'L 1000 100 Z'
        return re
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
