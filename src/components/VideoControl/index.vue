<!-- 视频播放控制组件 -->
<template>
  <div class="video-control">

    <!-- 视频主体 -->
    <video ref="video" src="https://www.w3school.com.cn/i/movie.ogg"
           @canplay="initVideo"
           @timeupdate="refreshCurrentTime">
    </video>

    <!-- 控制组件 -->
    <div class="control-container">

      <!-- 控制按钮 -->
      <div class="btn-container">

        <!-- 播放/暂停 按钮 -->
        <div v-if="!playStatus" class="play-btn" @click="playVideo"></div>
        <div v-else class="pause-btn" @click="pauseVideo"></div>

        <!-- 时间显示 -->
        <div class="timer">
          {{currentTime}} / {{duration}}
        </div>

        <!-- 音量调节按钮 -->
        <div class="volume-button">
          <input type="range" step="0.01" v-model="volume" @input="changeVolume"/>
        </div>
      </div>

      <!-- 播放进度条 -->
      <div class="progress-container">
        <input type="range" step="0.01" v-model="timeProcess" @input="changeTimeProcess"
               :style="{backgroundSize: timeProcess+'%' +' 100%'}">
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'VideoControl',
    data () {
      return {
        playStatus: false, // 是否正在播放
        currentTime: '00:00', // 当前播放时间
        duration: '00:00', // 视频总时长
        timeProcess: 0, // 当前播放进度占总长的的百分比
        volume: 60 // 当前音量百分比

      }
    },
    methods: {
      // 初始化视频相关信息
      initVideo () {
        this.duration = this.secondToMinute(this.$refs.video.duration)
      },

      // 开始播放视频
      playVideo () {
        this.$refs.video.play()
        this.playStatus = true
      },

      // 暂停播放视频
      pauseVideo () {
        this.$refs.video.pause()
        this.playStatus = false
      },

      // 视频播放中刷新当前播放时间与进度条
      refreshCurrentTime () {
        this.currentTime = this.secondToMinute(this.$refs.video.currentTime)
        this.timeProcess = (this.$refs.video.currentTime / this.$refs.video.duration).toFixed(4) * 100
      },

      // 拖动进度条修改当前进度
      changeTimeProcess () {
        this.$refs.video.currentTime = (this.$refs.video.duration * this.timeProcess) / 100
      },

      // 拖动声音滑块调整音量
      changeVolume () {
        this.$refs.video.volume = this.volume / 100
      },

      // 秒数转为分钟时间显示
      secondToMinute (t) {
        let m = Math.floor(t / 60) + ''
        let s = Math.floor(t % 60) + ''
        m = m.length === 1 ? '0' + m : m
        s = s.length === 1 ? '0' + s : s
        return m + ':' + s
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url(./index.less);
</style>
