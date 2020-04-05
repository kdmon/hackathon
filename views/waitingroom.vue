<template>
  <div class="row">
    <div class="col-xl-1 col-lg-2 col-md-3 col-sm-12 bg-black text-white">
      <h1 style="margin: 0.25em;">Väntrum</h1>
      
      <video id="localVideo"></video>
        
      <q-btn label="Avboka" @click="cancel('hej')"></q-btn> 
      
      <q-btn label="dela skärm" @click="shareScreen"></q-btn>
      
      <q-btn label="Lägg på" @click="closeConnection"></q-btn>
      
    </div>
    <div class="col-xl-11 col-lg-10 col-md-9 col-sm-12" id="splash">
      
      <q-btn fab icon="arrow_back" color="black" class="q-ma-lg" @click="closeConnection"></q-btn>
      
      <q-card class="shadow-10 text-white q-ml-lg" style="border: 1px solid #555; background: #0000004f; max-width: 450px; ccursor: pointer;" @oonclick="startMeeting">
        <q-card-section>
          <div style="width:50px; display: inline-block; vertical-align: top;">
            <img style="border-radius: 100%; width: 48px" src="https://randomuser.me/api/portraits/men/9.jpg">
          </div>
          <div style="display: inline-block">
            <h2 class="q-ma-none">Samtalsterapi</h2>
            <h3 class="q-my-none" sstyle="margin-left:2em">
              10:00-10:45
            </h3>
            <br>
            <p v-if="connecting">
              Väntar på att Morris Stewart ska starta mötet...
            <!--<q-btn fab icon="call" color="lime-7"></q-btn>-->
            </p>
            <p v-else>Du har inte anslutit till mötet ännu.</p>
          </div>
        </q-card-section>
        <q-separator color="grey-8"></q-separator>
        <q-card-actions align="between">
          <q-btn color="white" outline no-caps text-color="white" label="Lämna återbud" @click="cancel('hej')"></q-btn> 
          <q-btn v-if="!connecting" color="white" no-caps text-color="black" label="Anslut till mötet" unelevated @click="connecting = true"></q-btn> 
          <q-btn v-else color="green-3" no-caps text-color="black" label="Ansluter..." unelevated @click="joinRoom()">
          </q-btn> 
        </q-card-actions>
      </q-card>
      <div id="remoteVideos"></div>

      <!--
      <q-btn fab icon="call" color="lime-7"></q-btn>
      -->
      
    </div>
    
    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <h2 style="font-weight: 100; margin: 0" class="text-white">{{ currentTime }}</h2>
      <h3 style="font-weight: 100; margin: 0" class="text-white">{{ currentDate }}</h3>
    </q-page-sticky>
  </div>
</template>

<script>
  module.exports = {
    data: function () {
      return {
        connecting: false,
        currentTime: '',
        currentDate: '',
        readyToCall: false
      }
    },
    mounted: async function () {
      let d = new Date()
      setTimeout(() => this.startMeeting(), 4000)
      setInterval(() => this.currentDate =  new Date().toLocaleDateString(), 1000)
      setInterval(() => this.currentTime =  new Date().toLocaleTimeString(), 1000)
      Vue.prototype.$webrtc = await new SimpleWebRTC({
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
              maxFrameRate: 15,
              maxWidth: 320,
              maxHeight: 240
            }
          },
          audio: true
        }
      });
      
      this.$webrtc.on('readyToCall', () => {
        this.readyToCall = true
      })
    },
    methods: {
      closeConnection : function (room) {
        window.location = 'https://webappeditor.com/kdmon/hackathon/master/index.html#/'
        location.reload()
      },
      startMeeting : function () {
        //alert ("Mötet startar")
      },
      joinRoom : function (room) {
        connecting = false
        if (this.readyToCall) {
          setTimeout(() => {
            this.$webrtc.joinRoom(room)
            $router.push({ name: 'chat', params: { room: 'samtalsterapi' }})
          }, 4000)
        }
      },
      shareScreen : function () {
        if (this.$webrtc.getLocalScreen()) {
          this.$webrtc.stopScreenShare()
        } else {
        this.$webrtc.shareScreen (function (e) {
          console.log(e)
        })
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
</style>
  