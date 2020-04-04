<template>
  <div>
    <h1>Väntrummet</h1>
    
    <div id="localmedia"></div>
    <div id="remotemedia"></div>
    
    <div id="remoteVideos">
      <video id="localVideo"></video>
    </div>
    
    {{ currentTime }}
  </div>
</template>

<script>
  module.exports = {
    data: function () {
      return {
        currentTime: 'holk'
      }
    },
    mounted: function () {
      let d = new Date()
      interval = setInterval(() => this.currentTime =  new Date().toLocaleTimeString(), 1000)
      this.joinRoom('hej')
    },
    methods: {
      joinRoom : function (room) {
        
        let webrtc
        let mediaEnabled = {}
        
        if (webrtc) {
          webrtc.stopLocalVideo();
          webrtc.leaveRoom();
          webrtc.connection.disconnect();
          mediaEnabled.leftRoom = true;
          mediaEnabled.webrtc = false;
        }
        
        webrtc = new SimpleWebRTC({
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
        mediaEnabled.audio = "unmuted"
        mediaEnabled.video = "live"

        webrtc.on('readyToCall', function() {
          webrtc.joinRoom(room);
          mediaEnabled.webrtc = true;
          
          /*
          setTimeout(function () {
            $("#largeVideo").show().attr({
              "src": $("#localVideo").attr("src"),
              "autoplay": "autoplay"
            });
      
            // New video copying ...
            $("#largeVideo")[0].srcObject = $("#localVideo")[0].srcObject;
            $("#largeVideo")[0].muted = true;
      
            $("#innerVideoContainer").on('click', function() {
              $("#largeVideoContainer").hide();
              $("#chathistory").css("height","60%");
            });
            
            $("#innerVideoContainer").on('mouseover', function() {
              $("#hidelargevideo").show();
            });
            
            $("#innerVideoContainer").on('mouseout', function() {
              $("#hidelargevideo").hide();
            });     
            $("#remoteVideos").on('click', function (e) {
       
              $("#chathistory").css("height","25%");
              $("#largeVideoContainer").show();
              $("#largeVideo").show().attr({
                "src": $(e.target).attr("src"),
                "autoplay": "autoplay"
              });
              
              // New video copying ...
              $("#largeVideo")[0].srcObject = $(e.target)[0].srcObject;
              $("#largeVideo")[0].muted = true;
            });
          }, 2000);
          */
          
          //$("#mediacontainer").show();
          //$("#mediabuttonstop").show();
          //$("#mediabuttonleave").hide();
        })
      
      }
    }
  }
</script>