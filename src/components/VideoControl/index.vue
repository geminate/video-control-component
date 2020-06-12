<!-- 视频播放组件 -->
<template>
  <div class="video-control" @mousemove="initTimer" @touchstart="initTimer" @click="initTimer" ref="videoPage">

    <!-- 视频主体 -->
    <video ref="video" :src="src"
           @canplay="initVideo"
           @timeupdate="refreshCurrentTime"
           @play="onPlay"
           @pause="onPause"
    >
    </video>

    <!-- 控制组件 -->
    <transition name="fade">
      <div class="control-container" v-show="showController">

        <!-- 播放/暂停 按钮 -->
        <div class="play-btn" :class="{'pause':playStatus}" @click="togglePlay"></div>

        <!-- 时间显示 -->
        <div class="timer">
          {{currentTime}} / {{duration}}
        </div>

        <!-- 播放进度条 -->
        <div class="progress-container" @click="onSliderClick">
          <el-slider v-model="timeProcess" :step="0.00001" :show-tooltip="false" @input="onSliderChange"
                     ref="slider"></el-slider>
        </div>

        <!-- 倍速按钮 -->
        <el-dropdown ref="speedDrop" class="speed-btn" trigger="click" @command="changeSpeed">
          <span>倍速</span>
          <el-dropdown-menu class="speed-dropdown-menu" slot="dropdown">
            <el-dropdown-item command="0.5" :class="{active:playSpeed === 0.5}">0.5X</el-dropdown-item>
            <el-dropdown-item command="0.75" :class="{active:playSpeed === 0.75}">0.75X</el-dropdown-item>
            <el-dropdown-item command="1" :class="{active:playSpeed === 1}">1.0X</el-dropdown-item>
            <el-dropdown-item command="1.5" :class="{active:playSpeed === 1.5}">1.5X</el-dropdown-item>
            <el-dropdown-item command="2" :class="{active:playSpeed === 2}">2.0X</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>

        <!-- 音量调节按钮 -->
        <el-dropdown ref="volumeDrop" class="volume-button" :class="{mute:volume === 0}" trigger="click">
          <span></span>
          <el-dropdown-menu class="volume-dropdown-menu" slot="dropdown">
            <el-dropdown-item>
              <p>{{volume}}%</p>
              <el-slider v-model="volume" vertical :show-tooltip="false"></el-slider>
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>

        <!-- 全屏按钮 -->
        <div class="full-screen-btn" :class="{'is-full':isFullScreen}" @click="toggleFullScreen"></div>
      </div>
    </transition>
  </div>
</template>

<script>
  export default {
    name: 'videoPage',
    props: ['src'],
    data () {
      return {
        videoUrl: '', // 视频播放地址
        playStatus: false, // 是否正在播放
        currentTime: '00:00', // 当前播放时间
        duration: '00:00', // 视频总时长
        timeProcess: 0, // 当前播放进度占总长的的百分比
        playSpeed: 1, // 当前播放速度
        volume: 100, // 当前音量百分比
        isFullScreen: false, // 是否全屏
        timer: null, // 隐藏控件定时器
        showController: true, // 是否展示 控制栏
        lastMousePosition: [0, 0], // 上一次 mousemove 位置
        lastMoveTime: 0 // 鼠标移动节流
      }
    },
    watch: {

      // 隐藏控制栏同时隐藏全部的下拉菜单
      showController (val) {
        if (!val) {
          this.$refs.speedDrop.hide()
          this.$refs.volumeDrop.hide()
        }
      },

      // 修改播放音量
      volume (val) {
        this.$refs.video.volume = val / 100
      }
    },
    methods: {

      // 页面初始化
      init () {
        this.initTimer()
        this.initKeyPress()
      },

      // 重置/初始化 隐藏控件定时器
      initTimer (e) {
        if (e && e.type === 'mousemove') {
          this.showController = true
          this.timer && clearTimeout(this.timer)
          this.timer = setTimeout(() => {
            this.showController = false
          }, 5000)
          this.lastMoveTime = new Date()
        }
      },

      // 初始化 ESC 按钮按下事件
      initKeyPress () {
        document.addEventListener('keyup', this.onKeyPress)
      },

      // 按钮按下事件: ESC、上下、左右、空格
      onKeyPress ({ keyCode }) {
        this.initTimer()
        if (keyCode === 27) { // ESC 按钮
          this.exitFullscreen()
        } else if (keyCode === 32) { // 空格
          this.togglePlay()
        } else if (keyCode === 37) { // 左箭头
          this.changeTimeProcess(this.timeProcess >= 10 ? (this.timeProcess - 10) : 0)
        } else if (keyCode === 39) { // 右箭头
          this.changeTimeProcess(this.timeProcess <= 90 ? (this.timeProcess + 10) : 100)
        } else if (keyCode === 38) { // 上箭头
          this.$refs.volumeDrop.show()
          this.volume <= 90 ? this.volume += 10 : this.volume = 100
        } else if (keyCode === 40) { // 下箭头
          this.$refs.volumeDrop.show()
          this.volume >= 10 ? this.volume -= 10 : this.volume = 0
        }
      },

      // 初始化视频相关信息
      initVideo () {
        this.duration = this.secondToMinute(this.$refs.video.duration)
      },

      // 播放/暂停 切换
      togglePlay () {
        if (this.playStatus) {
          this.$refs.video.pause()
        } else {
          this.$refs.video.play()
        }
        this.playStatus = !this.playStatus
      },

      // 进度条点击修改进度
      onSliderClick () {
        this.$refs.video.currentTime = (this.$refs.video.duration * this.timeProcess) / 100
      },

      // 进度条拖动修改进度
      onSliderChange () {
        if (this.$refs.slider.dragging) { // 只有拖动过程中才实时修改播放进度
          this.$refs.video.currentTime = (this.$refs.video.duration * this.timeProcess) / 100
        }
      },

      // 视频播放中刷新当前播放时间与进度条
      refreshCurrentTime () {
        this.currentTime = this.secondToMinute(this.$refs.video.currentTime)
        this.timeProcess = (this.$refs.video.currentTime / this.$refs.video.duration) * 100
      },

      // 拖动进度条修改当前进度
      changeTimeProcess (val) {
        this.timeProcess = val
        this.$refs.video.currentTime = (this.$refs.video.duration * val) / 100
      },

      // 调整播放速度
      changeSpeed (playSpeed) {
        this.playSpeed = Number(playSpeed)
        this.$refs.video && (this.$refs.video.playbackRate = Number(playSpeed))
      },

      // 全屏切换
      toggleFullScreen () {
        if (this.isFullScreen) {
          this.exitFullscreen()
        } else {
          this.launchFullscreen()
        }
      },

      // 视频开始播放回调
      onPlay () {
        this.playStatus = true
      },

      // 视频被暂停回调
      onPause () {
        this.playStatus = false
      },

      // 秒数转为分钟时间显示
      secondToMinute (t) {
        let m = Math.floor(t / 60) + '', s = Math.floor(t % 60) + ''
        m = m.length === 1 ? '0' + m : m
        s = s.length === 1 ? '0' + s : s
        return m + ':' + s
      },

      // 进入全屏
      launchFullscreen () {
        this.isFullScreen = true
      },

      // 退出全屏
      exitFullscreen () {
        this.isFullScreen = false
      }
    },
    mounted () {
      this.init()
    },
    beforeDestroy () {
      this.timer && clearTimeout(this.timer)
      document.removeEventListener('keyup', this.onKeyPress)
    }
  }
</script>

<style lang="less" scoped>
  @import url("index.less");
</style>
