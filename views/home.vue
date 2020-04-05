<template>
  <div id="splash" class="q-pa-lg">
    <q-card class="q-ma-lg faded" style="margin: 2em auto 0 auto; max-width: 1024px">
      <q-card-section>
        <div class="row q-col-gutter-xl">
          <div class="col-xs-12 col-sm-6 col-md-6">
            <h1 class="sscript text-black" style="ffont-weight: 100; margin-top: 0">
              Konfidentiella gruppsamtal på distans
            </h1>
            <p style="font-size: 1.5em">
              Ett säkert, tryggt och bekvämt sätt att tillsammans 
              förbättra din och dina medmänniskors  hälsa.
            </p>
          </div>
          <div class="col-xs-12 col-sm-6 col-md-6">
            <img src="https://images.unsplash.com/photo-1490111718993-d98654ce6cf7?ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80" style="width: 100%">
            <iframe style="display:none; width: 100%; height: 320px; max-width: 550px;" src="https://www.youtube.com/embed/bsJ_5E1JEIU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
          </div>
        </div>
      </q-card-section>
      <q-card-section>
        <div class="row q-col-gutter-xl">
          <div class="col-sm-12 col-md-6">
            <p>
              För att använda tjänsten behöver du ha tillgång till en dator
              eller Android enhet med Chrome, Firefox eller Edge webbläsare.
            </p>
          </div>
          <div class="col-sm-12 col-md-6">
            <q-btn
              size="24px"
              unelevated
              icon="account_circle"
              no-caps
              label="Logga in med BankId"
              color="green-6" 
              text-color="white"
              to="/waitingroom">
            </q-btn>
            
          </div>
        </div>
      </q-card-section>
    </q-card>
    
    <q-card class="q-mt-xl" style="max-width:800px; margin: 3em auto 0 auto;">
      <q-card-section>
        <h2 class="script">Vanliga frågor</h2>
        
        <h3>Vad är detta?</h3>
        <p>
          Detta är en snabb proof-of-concept, skapad under Hackthecrisis
          4-6 april 2020.
        </p>

        <h3>Lagrar eller lyssnar ni på mina samtal?</h3>
        <p>
          Nej. Tjänsten bygger på sk. decentraliserad "peer to peer" teknik,
          det innebär att informationen i ditt videosamtal aldrig passerar
          våra servrar, utan skickas enbart till deltagarna i ditt samtal.
          All kommunikation är också enkrypterad.
        </p>

        <div class="row">
          <div class="col-6">
            <h3>Hur fungerar det rent tekniskt?</h3>
            <p>
              Deltagare autentiserar sig genom en sk. IdP via t.ex. BankId. För
              att få tillgång till ett samtalsrum krävs dessutom en unik kod
              som kan skickas ut till deltagare via t.ex. e-post eller SMS.
            </p>
            <p>När en deltagare ansluter sig till ett samtal kommunicerar de först
              med en central server (sk. signaling server). När samtalet är kopplat
              bryts kontakten med servern. 
            </p>
            <p>
              Videosamtalen använder en standard som heter WebRTC. Den tekniska
              lösningen bygger på två tekniska ramverk: SimpleWebRTC och signal-master.
            </p>
          </div>
          <div class="col-6">
            <img src="https://webrtc-security.github.io/images/diagram_4_en.png" style="width: 100%; padding: 1em;">
            <p>
              Källa: <a href="https://webrtc-security.github.io/" target="_blank">A Study of WebRTC Security</a>
            </p>
          </div>
        </div>
        
        <h3>Finns koden någonstans?</h3>
        <p>
          Ja, den finns att hämta på 
          <a href="https://github.com/kdmon/hackathon">Github</a>.
        </p>

      </q-card-section>
    </q-card>
  </div>
</template>

<style>
  #splash {
    background: url(https://images.unsplash.com/photo-1524865291454-215bdc75819e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1868&q=80);
    background-size: cover;
    min-height: calc(100vh - 50px);
  }
</style>