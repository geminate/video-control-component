<!-- 视频播放组件 -->
<template>
  <div class="video-control" @mousemove="initTimer" @touchstart="initTimer" @click="initTimer" ref="videoControl">
    <!-- 视频主体 -->
    <video ref="video" :src="src"
           @durationchange="onDurationChange"
           @timeupdate="onTimeUpdate"
           @play="onPlay"
           @pause="onPause"
    >
    </video>

    <!-- 控制组件 -->
    <transition name="fade">
      <div class="control-container" v-show="showController">
        <play-btn class="play-btn" v-model="playStatus"></play-btn>
        <progress-bar class="progress-bar" ref="progressBar" v-model="timeProcess"
                      @click="setCurrentTime"></progress-bar>
        <speed-btn class="speed-btn" v-model="playSpeed"></speed-btn>
        <volume-btn class="volume-button" v-model="volume"></volume-btn>
        <full-screen-btn class="full-screen-btn" ref="fullScreenBtn" v-model="isFullScreen"></full-screen-btn>
      </div>
    </transition>
  </div>
</template>

<script>
  import PlayBtn from './PlayBtn'
  import ProgressBar from './ProgressBar'
  import SpeedBtn from './SpeedBtn'
  import VolumeBtn from './VolumeBtn'
  import FullScreenBtn from './FullScreenBtn'

  export default {
    name: 'videoPage',
    components: { PlayBtn, ProgressBar, SpeedBtn, VolumeBtn, FullScreenBtn },
    props: ['src'],
    data () {
      return {
        showController: true, // 是否显示控制栏
        timer: null, // 控制栏显隐计时器

        duration: 0, // 视频总秒数
        currentTime: 0, // 当前播放秒数
        playStatus: false, // 视频是否正在播放
        isFullScreen: false, // 当前是否是全屏播放
        volume: 80, // 音量大小
        playSpeed: 1, // 播放速率
        timeProcess: 0 // 当前播放进度占总长的的百分比
      }
    },
    watch: {
      isFullScreen () {
        this.$refs.fullScreenBtn.toggleFullScreen(this.$refs.videoControl)
      },
      playStatus (val) {
        val ? this.$refs.video.play() : this.$refs.video.pause()
      },
      volume (val) {
        this.$refs.video.volume = val / 100
      },
      playSpeed (val) {
        this.$refs.video.playbackRate = val
      },
      timeProcess (val) {
        if (this.$refs.progressBar.isDragging()) {
          this.setCurrentTime()
        }
      }
    },
    methods: {
      initTimer () {
        this.showController = true
        this.timer && clearTimeout(this.timer)
        this.timer = setTimeout(() => {
          this.showController = false
        }, 5000)
      },
      onDurationChange () {
        this.duration = this.$refs.video.duration
      },
      onTimeUpdate () {
        this.currentTime = this.$refs.video.currentTime
        this.timeProcess = (this.$refs.video.currentTime / this.$refs.video.duration) * 100
      },
      onPlay () {
        this.playStatus = true
      },
      onPause () {
        this.playStatus = false
      },
      setCurrentTime () {
        this.$refs.video.currentTime = (this.$refs.video.duration * this.timeProcess) / 100
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
