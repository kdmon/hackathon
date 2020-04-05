<template>
  <div id="splash" class="q-pa-lg">
    <q-card class="q-ma-lg faded" style="margin: auto; max-width: 1024px">
      <q-card-section>
        <div class="row q-col-gutter-xl">
          <div class="col-xs-12 col-sm-6 col-md-6">
            <h1 class="script text-blue" style="font-weight: 700; margin-top: 0">Gruppsamtal med din terapeut på webben </h1>
              <p style="font-size: 1.5em">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi nec
              odio orci. Nam interdum mi sit amet turpis pellentesque aliquam. 
            </p>
          </div>
          <div class="col-xs-12 col-sm-6 col-md-6">
            <img src="https://images.unsplash.com/photo-1546387903-6d82d96ccca6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1051&q=80" style="width: 100%">
            <iframe style="display:none; width: 100%; height: 320px; max-width: 550px;" src="https://www.youtube.com/embed/bsJ_5E1JEIU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
          </div>
        </div>
      </q-card-section>
      <q-card-section>
        <div class="row q-col-gutter-xl">
          <div class="col-sm-12 col-md-6">
            <p>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi nec
              odio orci. Nam interdum mi sit amet turpis pellentesque aliquam. Proin
              imperdiet sit amet ante id lobortis. Aliquam a augue sem. Mauris in 
              laoreet diam. Cras orci metus, tempus et hendrerit eleifend, faucibus
              eu libero. Pellentesque ac vulputate nisi. Aenean vel bibendum ante. 
              Pellentesque fermentum felis eu tortor imperdiet lacinia. Nullam 
              pulvinar eros id nisi ullamcorper faucibus. In id lorem eu ligula 
              fringilla elementum nec in felis. Praesent auctor nulla lectus, sit
              amet tempus leo luctus ac.
            </p>
          </div>
          <div class="col-sm-12 col-md-6">
            <q-btn
              size="24px"
              unelevated
              no-caps
              label="Logga in med BankId"
              color="lime-5" 
              text-color="black"
              to="/waitingroom">
            </q-btn>
            <h3 class="script q-qmt-sm">Smidigt, tryggt och säkert.</h3>
          </div>
        </div>
      </q-card-section>
    </q-card>
  </div>
</template>

<style>
  #splash {
    background: url(https://images.unsplash.com/photo-1490111718993-d98654ce6cf7?ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80);
    background-size: cover;
    min-height: calc(100vh - 50px);
  }
</style>