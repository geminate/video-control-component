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
  @import url("./assets/css/main.less");

  .video-control-container {
    width: 1050px;
    height: 700px;
  }
</style>
