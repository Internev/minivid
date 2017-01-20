<template>
     <div>
      <videocomponent id="video" :wsrtc="wsrtc" :uri="uri"></videocomponent>
     </div>
</template>

<script>
  import Videocomponent from './video_component.vue'

  export default {
    created() {
      let c = this.$route.params.channel
      this.uri = c !== undefined && /^\w{5}$/.test(c) ? c : 'bread'
      //create RTC websocket
      this.wsrtc = new WebSocket(`wss://${window.location.host}/ws/${this.uri}rtc`);
      // update URL display. I still think we can do this with router somehow :S
      window.history.pushState(window.location.origin, '/', this.uri);

    },
    data() {
      return {
        ws: null,
        wsrtc: null,
        channel: ''      }
    },
    components: {
      Videocomponent
    }
  }
</script>

<style scoped>
.main-content{
  width: 100vw;
}
.content{
  display: inline-flex;
  height: 87vh;
  width: 100vw;
}
.content-right{
  width: 80%;
  display: inline-block;
  float: right;
}
.content-left{
  width: 20%;
  display: inline-block;
  justify-content: center;
  align-items: flex-end;
}
.doc-info{
  font-size: 0.95em;
  font-weight: 600;
  margin: 0.75em;
  opacity: 0.35;
}
html, body{
  color: #333;
  font-family: 'Monaco', courier, monospace;
}
#editor {
  height: 100%;
}
code {
  color: #f66;
}
</style>
