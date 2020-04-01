<template>
  <div id="app">
    <video-control></video-control>
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
</style>
