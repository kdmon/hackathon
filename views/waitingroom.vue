<template>
  <div class="row">
    <div class="col-xl-2 col-lg-3 col-md-3 col-sm-12 bg-black text-white">
      <!--<h3 style="margin: 0.25em; font-size: 2em">Inställningar</h3>-->

      <br>
      
      <video id="localVideo"></video>
      
      <div v-if="connected" id="controls" style="display: flex; align-items: center; justify-content: center; margin-top: -7em;">
        <q-btn v-if="micOff" @click="micOff = false" fab color="blue" class="q-ma-xs" icon="mic_off">
          <q-tooltip>Sätt på mikrofonen</q-tooltip>
        </q-btn>
        
        <q-btn v-if="!micOff" @click="micOff = true" fab color="white" class="q-ma-xs" outline icon="mic">
          <q-tooltip>Stäng av mikrofonen</q-tooltip>
        </q-btn>

        <q-btn fab v-if="shareVideo" @click="shareVideo = false" color="white" class="q-ma-xs" outline icon="videocam">
          <q-tooltip>Sluta dela video</q-tooltip>
        </q-btn>
        
        <q-btn fab v-if="!shareVideo" @click="shareVideo = true" color="blue" class="q-ma-xs" icon="videocam_off">
          <q-tooltip>Börja dela video</q-tooltip>
        </q-btn>
        
        <q-btn fab v-if="!screenSharing" @click="shareScreen()" color="blue" class="q-ma-xs" icon="stop_screen_share">
          <q-tooltip>Börja dela skärm<br>(Kräver Firefox)</q-tooltip>
        </q-btn>
        
        <q-btn fab v-if="screenSharing" @click="shareScreen(true)" color="white" class="q-ma-xs" outline icon="screen_share">
          <q-tooltip>Sluta dela skärm</q-tooltip>
        </q-btn>
        
        <q-btn fab @click="closeConnection" color="red" class="q-ma-xs" icon="call_end">
          <q-tooltip>Lämna samtalet</q-tooltip>
        </q-btn>
      
      </div>

    </div>
    <div class="col-xl-10 col-lg-9 col-md-9 col-sm-12" id="splash">
      
      <div v-if="!connected">
        <q-btn label="Avsluta och lämna väntrummet" no-caps icon="arrow_back" unelevated text-color="white" class="q-ma-lg" @click="leaveRoom"></q-btn>
        
        <!--<h1 class="text-white q-ma-lg">Mina samtal</h1>-->
        
        <q-card class="shadow-10 text-white q-ma-lg" style="border: 1px solid #555; background: #0000004f; max-width: 450px; ccursor: pointer;" @oonclick="startMeeting">
          <q-card-section>
            <div style="width:50px; display: inline-block; vertical-align: top;">
              <img style="border-radius: 100%; width: 48px" src="https://randomuser.me/api/portraits/men/9.jpg">
              <q-tooltip>Anders Frisk<br>Leg. psykolog</q-tooltip>
            </div>
            <div style="display: inline-block">
              <h2 class="q-ma-none">Samtalsterapi</h2>
              <h3 class="q-my-none" sstyle="margin-left:2em">
                Idag 10:00-10:45
              </h3>
              <br>
              <p v-if="connected">
                Väntar på att samtalet ska starta ... <q-spinner></q-spinner>
              <!--<q-btn fab icon="call" color="lime-7"></q-btn>-->
              </p>
              <p v-else>Du har inte anslutit till samtalet ännu.</p>
            </div>
          </q-card-section>
          <q-separator color="grey-8"></q-separator>
          <q-card-actions align="between">
            <q-btn color="white" outline no-caps text-color="white" label="Lämna återbud" @click="cancel('hej')"></q-btn> 
            <q-btn v-if="readyToCall" color="green-6" no-caps text-color="white" icon="call" label="Anslut till samtalet" unelevated @click="joinRoom('samtalsterapi')"></q-btn> 
            <!-- <q-btn v-else color="green-3" no-caps text-color="black" label="Ansluter..." unelevated @click="leaveRoom('samtalsterapi')"></q-btn> -->
          </q-card-actions>
        </q-card>
      </div>
      
      <div v-else>
        <div id="remoteVideos" :class="'feeds-' + feeds"></div>
      </div>

      <!--
      <q-btn fab icon="call" color="lime-7"></q-btn>
      -->
      
    </div>
    
    <q-page-sticky position="bottom-right" :offset="[18, 18]" v-if="!connected"> 
      <h2 style="font-weight: 100; margin: 0" class="text-white">{{ currentTime }}</h2>
      <h3 style="font-weight: 100; margin: 0" class="text-white">{{ currentDate }}</h3>
    </q-page-sticky>
  </div>
</template>

<script>
  module.exports = {
    data: function () {
      return {
        micOff: false,
        shareVideo: true,
        screenSharing: false,
        connected: false,
        currentTime: '',
        currentDate: '',
        readyToCall: false,
        feeds: 0
      }
    },
    mounted: async function () {
      let d = new Date()
      setTimeout(() => this.startMeeting(), 4000)
      setInterval(() => this.currentDate =  new Date().toLocaleDateString(), 1000)
      setInterval(() => this.currentTime =  new Date().toLocaleTimeString(), 1000)
      setInterval(() => this.checkFeeds(), 500)
      this.initCamera()
    },
    methods: {
      closeConnection : function (room) {
        //window.location = 'https://webappeditor.com/kdmon/hackathon/master/index.html#/waitingroom'
        location.reload()
      },
      checkFeeds : function () {
        let e = document.getElementById('remoteVideos')
        if (e) this.feeds = e.childElementCount
      },
      startMeeting : function () {
        //alert ("Mötet startar")
      },
      leaveRoom : function () {
        window.location = 'https://webappeditor.com/kdmon/hackathon/master/index.html'
      },
      initCamera : function () {
        Vue.prototype.$webrtc = new SimpleWebRTC({
          url: "https://webappeditor.com:8085",
          socketio: {'force new connection': true},
          localVideoEl: 'localVideo',
          remoteVideosEl: 'remoteVideos',
          autoRequestMedia: true,
          media: {
            video: {
              mandatory: {
                //chromeMediaSource: 'desktop',
  			        //chromeMediaSourceId: 'screen',
                maxFrameRate: 30
                //maxWidth: 320,
                //maxHeight: 240
              }
            },
            audio: true
          }
        });
        
        this.$webrtc.on('readyToCall', () => {
          this.readyToCall = true
        })
  
      },
      joinRoom :  function (room) {
        this.connected = true
        this.$webrtc.joinRoom(room)
      },
      shareScreen : function (stop) {
        if (this.$webrtc.getLocalScreen() || stop) {
          this.$webrtc.stopScreenShare()
          this.screenSharing = false
        } else {
          this.$webrtc.shareScreen(function (e) {
            console.log(e)
          })
          this.screenSharing = true
        }
      }
    }
  }
</script>


<style>
  #splash {
    background: url(https://images.unsplash.com/photo-1518241353330-0f7941c2d9b5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1850&q=80);
    background-size: cover;
    min-height: calc(100vh - 50px);
  }
  #localVideo {
    width: 100%;
    border: 1em solid black;
    border-radius: 2em;
  }
  
  #remoteVideos {
    display: flex;
    width: 100%;
    height: calc(100vh - 50px);
    background: #aaa;
  }
  
  .feeds-1 video {
    width: 100%;
    height: 100%;
  }
  
  .feeds-2 video {
    width: 50%;
    height: 100%;
  }
  
  .feeds-3 video {
    width: 33%;
    height: 100%;
  }
  
  .feeds-4 video {
    width: 50%;
    height: 50%;
  }
  
  .feeds-5 video {
    width: 33%;
    height: 50%;
  }
  
  .feeds-6 video {
    width: 33%;
    height: 50%;
  }
  
  .feeds-7 video {
    width: 33%;
    height: 33%;
  }

  .feeds-8 video {
    width: 33%;
    height: 33%;
  }

  .feeds-9 video {
    width: 33%;
    height: 33%;
  }
  
  #controls {
    opacity: 0.8;
  }

</style>
  