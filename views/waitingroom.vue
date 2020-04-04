<template>
  <div>
    <h1>Väntrummet</h1>
    
    <div id="localmedia"></div>
    <div id="remotemedia"></div>
    
    <q-btn label="ring upp" @click="joinRoom"></q-btn>
    
    <q-btn label="dela skärm" @click="shareScreen"></q-btn>
    
    <div id="remoteVideos">
      <video id="localVideo"></video>
    </div>
    
    <h3>{{ currentTime }}</h3>
    
  </div>
</template>

<script>
  module.exports = {
    data: function () {
      return {
        currentTime: '',
        readyToCall: false,
        webrtc : {}
      }
    },
    mounted: function () {
      let d = new Date()
      interval = setInterval(() => this.currentTime =  new Date().toLocaleTimeString().substring(0,5), 1000)
      this.webrtc = new SimpleWebRTC({
        url: "https://webappeditor.com:8085",
        socketio: {'force new connection': true},
        localVideoEl: 'localVideo',
        remoteVideosEl: 'remoteVideos',
        autoRequestMedia: true,
        media: {
          video: {
            mandatory: {
              maxFrameRate: 15,
              maxWidth: 320,
              maxHeight: 240
            }
          },
          audio: true
        }
      });
      this.webrtc.instance.on('readyToCall', function() {
        this.readyToCall = true
      })
    },
    methods: {
      joinRoom : function (room) {
        if (this.readyToCall) {
          this.webrtc.joinRoom(room)
        } else {
          alert ("Not ready to call")
        }
      },
      shareScreen : function () {
        if (this.webrtc.instance.getLocalscreen()) {
          this.webrtc.instance.stopScreenShare()
        } else {
        this.webrtc.instance.shareScreen (function (e) {
          console.log(e)
        })
        }
      }
    }
  }
</script>