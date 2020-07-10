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
      <v-card class="ma-5">
      <v-container>
    <v-row justify="center">
     <v-card>
        <v-text-field placeholder="enter address" v-model="address" />
        <v-card-actions class="justify-center">
        <v-btn class="primary" justify="center" @click="getLocation">submit</v-btn>
      </v-card-actions>
      </v-card>
      <br/>
    </v-row>
    <br>
    <!-- <v-row justify="center"> -->
      <div id="map" ref="map">
      </div>
      <p>{{address}} {{ lat }} {{ lng}}</p>
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
      latlng: {lat: -25.344, lng: 131.036}
    }),
    methods: {
      getLocation() {
        const url = 'https://api.mapbox.com/geocoding/v5/mapbox.places/' + this.address + '.json?access_token=pk.eyJ1IjoidWR1ZGl0MDA3IiwiYSI6ImNrMWhpejZ4NDFhaDAzaHFtdndpejZlMXoifQ.B_k4N8sXB5F-HyK1sXi8qw'
        this.$http.get(url).then(response=>{
        if (response.body.features.length > 0){
                    this.lat=response.body.features[0].center[1];
                    this.lng=response.body.features[0].center[0];
                  }
                  else {
                    alert("entered address doesn't exist")
                  }
        },error=>{
          console.log("error", error)
        })


        
      }
    },
    updated() {
      this.map = new window.google.maps.Map(this.$refs["map"], {
        center: {lat: this.lat, lng: this.lng},
        zoom: 7
      })
      new window.google.maps.Marker({
        position: {lat: this.lat, lng: this.lng},
        map:  this.map
      })
     
    },
    mounted() {
      this.map = new window.google.maps.Map(this.$refs["map"], {
        center: {lat: this.lat, lng: this.lng}, 
        zoom: 7
      })
      new window.google.maps.Marker({
        position: {lat: this.lat, lng: this.lng},
        map:  this.map
      })
    }
  }
</script>

<style scoped>
  #map{
    height: 400px;
    text-align: center;
  }
</style>
