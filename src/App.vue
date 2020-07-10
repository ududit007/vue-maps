<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
     <v-toolbar-title class="white--text darken-4">Address validator</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-card class="ma-5" flat>
      <v-container>
    <v-row justify="center">
     <!-- <v-card> -->
      <!-- <div> -->
        <v-col xs="2" />
        <v-col xs="4">
        <v-text-field placeholder="enter address" prepend-icon="mdi-map-marker" v-model="address" />
      </v-col>
      <v-col xs="4">
         <v-btn icon>
        <v-icon @click="getLocation">mdi-magnify</v-icon>
      </v-btn>
    </v-col>
      <br/>
    </v-row>
    <br>
    <!-- <v-row justify="center"> -->
      <div id="map" ref="map" v-if="show">
      </div>
      <v-alert v-if="lng==0 && lat==0" type="error" dismissible  dense top elevation="2">
      your address is invalid
    </v-alert>

       <!-- </v-row> -->
        </v-container>
      </v-card>
    </v-content>
  </v-app>
</template>

<script>
export default {
    components: {
     
    },
    data: () => ({
      address: '',
      res: [0, 0],
      map: null,
      lat: -25.344,
      lng: 131.036,
      show: false
    }),
    methods: {
      async getLocation() {
        const url = 'https://api.mapbox.com/geocoding/v5/mapbox.places/' + this.address + '.json?access_token=pk.eyJ1IjoidWR1ZGl0MDA3IiwiYSI6ImNrMWhpejZ4NDFhaDAzaHFtdndpejZlMXoifQ.B_k4N8sXB5F-HyK1sXi8qw'
        await this.$http.get(url).then(response=>{
        if (response.body.features.length > 0){
                    this.lat=response.body.features[0].center[1];
                    this.lng=response.body.features[0].center[0];
                    this.show = true;
                  }
                  else {
                    this.show = false;
                    this.lat = 0;
                    this.lng = 0;
                  }
        },error=>{
          console.log("error", error)
        })

        this.map = new window.google.maps.Map(this.$refs["map"], {
        center: {lat: this.lat, lng: this.lng}, 
        zoom: 7
      })
      new window.google.maps.Marker({
        position: {lat: this.lat, lng: this.lng},
        map:  this.map
      })

        
      }
    },
    created() {
     
    },
    updated() {
      
     
    },
    
    mounted() {
      
    }
  }
</script>

<style scoped>
  #map{
    height: 400px;
    text-align: center;
  }
</style>
