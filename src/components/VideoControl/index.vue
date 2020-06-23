<!-- 视频播放组件 -->
<template>
  <div class="video-control" ref="videoControl">

    <div class="loading" v-show="showLoading"></div>

    <!-- 视频主体 -->
    <video ref="video"
           preload="auto"
           :src="src"
           @loadstart="onLoadStart"
           @durationchange="onDurationChange"
           @timeupdate="onTimeUpdate"
           @play="onPlay"
           @pause="onPause"
           @progress="onProgress"
           @playing="onPlaying"
           @waiting="onWaiting"
           @seeking="onSeeking"
           @seeked="onSeeked"
    >
    </video>

    <!-- 控制组件 -->
    <div class="control-container">
      <progress-bar class="progress-bar" :progress="timeProcess" @change="setCurrentTime"
                    :buffered="buffered" :playedDuration="playedDuration"></progress-bar>
      <play-btn class="play-btn" v-model="playStatus"></play-btn>
      <video-timer class="video-timer" :duration="duration" :currentTime="currentTime"></video-timer>
      <danmu-input class="danmu-input"></danmu-input>
      <speed-btn class="speed-btn" v-model="playSpeed"></speed-btn>
      <volume-btn class="volume-button" v-model="volume"></volume-btn>
      <full-screen-btn class="full-screen-btn" ref="fullScreenBtn" v-model="isFullScreen"></full-screen-btn>
    </div>
  </div>
</template>

<script>
  import PlayBtn from './PlayBtn'
  import VideoTimer from './VideoTimer'
  import DanmuInput from './DanmuInput'
  import ProgressBar from './ProgressBar'
  import SpeedBtn from './SpeedBtn'
  import VolumeBtn from './VolumeBtn'
  import FullScreenBtn from './FullScreenBtn'

  export default {
    name: 'videoPage',
    components: { DanmuInput, PlayBtn, VideoTimer, ProgressBar, SpeedBtn, VolumeBtn, FullScreenBtn },
    props: ['src'],
    data () {
      return {
        duration: 0, // 视频总秒数
        currentTime: 0, // 当前播放秒数
        playStatus: false, // 视频是否正在播放
        isFullScreen: false, // 当前是否是全屏播放
        volume: 80, // 音量大小
        playSpeed: 1, // 播放速率
        timeProcess: 0, // 当前播放进度占总长的的百分比
        buffered: 0,
        showLoading: false,
        playedDuration: {
          playedArray: [],
          playingDuration: [0, 0],
          startTemp: 0,
          endTemp: 0
        },
        isSeeking: false
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
      }
    },
    methods: {
      addPlayedArray (currentTime) {
        const x = this.playedDuration.startTemp / this.duration * 100
        const width = currentTime / this.duration * 100 - x
        this.playedDuration.playedArray.push([x, width])
      },
      refreshPlayingDuration () {
        const x = this.isSeeking ? (this.currentTime / this.duration * 100) : (this.playedDuration.startTemp / this.duration * 100)
        const width = this.currentTime / this.duration * 100 - x
        this.playedDuration.playingDuration = [x, width]
      },
      onSeeking () {
        this.isSeeking = true
        this.addPlayedArray(this.currentTime)
      },
      onSeeked () {
        this.isSeeking = false
        this.playedDuration.startTemp = this.currentTime
      },
      onLoadStart () {
        this.showLoading = true
      },
      onWaiting () {
        this.showLoading = true
      },
      onPlaying () {
        this.playedDuration.startTemp = this.currentTime
        this.showLoading = false
      },
      onDurationChange () {
        this.duration = this.$refs.video.duration
        this.setBuffered()
        this.showLoading = false
      },
      onTimeUpdate (a, b) {
        this.currentTime = this.$refs.video.currentTime
        this.timeProcess = (this.$refs.video.currentTime / this.$refs.video.duration) * 100
        this.refreshPlayingDuration()
      },
      onProgress () {
        this.setBuffered()
      },
      onPlay () {
        this.playStatus = true
      },
      onPause () {
        this.playStatus = false
      },
      setBuffered () {
        this.buffered = 0
        let length = this.$refs.video.buffered.length, re = 0
        while (length > 0 && this.$refs.video.buffered.end(length - 1) > this.currentTime) {
          re = this.$refs.video.buffered.end(length - 1)
          length--
        }
        this.buffered = this.duration === 0 ? 0 : re / this.duration * 100
      },
      setCurrentTime (val) {
        this.timeProcess = val
        this.$refs.video.currentTime = (this.$refs.video.duration * val) / 100
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
