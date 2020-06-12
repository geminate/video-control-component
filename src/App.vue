<template>
  <div id="app">
    <div class="video-control-container">
      <video-control src="https://www.w3school.com.cn/i/movie.ogg"></video-control>
    </div>
  </div>
</template>

<script>
  import VideoControl from './components/VideoControl'

  export default {
    name: 'App',
    components: {
      VideoControl
    }
  }
</script>

<style>
  @import "../static/reset.css";

  .video-control-container {
    width: 950px;
    height: 600px;
  }
</style>
