<template>

    <div class="content-left">
      <VideoComponent id="video" :wsRTC="wsRTC" :saverURI="saverURI"></VideoComponent>
      <!-- <ScreenCast id="screen" :wsRTC="wsRTC" :saverURI="saverURI"></ScreenCast> -->
    </div>

</template>

<script>

  import VideoComponent from './video_component.vue'

  export default {
    created() {
      // get params from URL (if provided)
      let c = this.$route.params.channel;
      // set URI to params or generated 5 char unique.
      // let URI = c !== undefined && /^\w{5}$/.test(c) ? c : 'bread';
      let URI = 'bread'
      this.saverURI = URI
      // create websocket with unique address.
      // this.ws = new WebSocket(`wss://${window.location.host}/ws/${this.URI}`);
      //create RTC websocket
      this.wsRTC = new WebSocket(`wss://${window.location.host}/ws/${this.URI}rtc`);
      //create RTC screencast websocket
      this.wsScreen = new WebSocket(`wss://${window.location.host}/ws/${this.URI}screen`);
      // update URL display. I still think we can do this with router somehow :S
      window.history.pushState(window.location.origin, '/', URI);

    },
    data() {
      return {
        saverURI: '',
        ws: null,
        wsRTC: null,
        wsScreen: null,
        answer:'',
        input: '',
        channel: '',
        count: 0,
        channel: ''
      }
    },
    components: {
      VideoComponent
      // ScreenCast
    },
  }
</script>

<style>
  .main-content{
    width: 100vw;
  }
  .content-right{
    border: 1px solid transparent;
    margin-left: 30vw;
    width: 70vw;
    height: 100vh;
  }
  .content-left{
    position: fixed;
    width: 30vw;
  }
  html, body, #editor {
    margin: 0;
    height: 100%;
    color: #333;
    font-family: 'Monaco', courier, monospace;
  }
  textarea, #editor div {
    display: inline-block;
    width: 100%;
    height: 100%;
    vertical-align: top;
    box-sizing: border-box;
    padding: 0 20px;
  }
  textarea {
    border: none;
    border-right: 1px solid #ccc;
    resize: none;
    outline: none;
    background-color: #f6f6f6;
    font-size: 14px;
    padding: 20px;
  }
  code {
    color: #f66;
  }
</style>
