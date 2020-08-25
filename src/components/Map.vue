<template>
  <div>
    <div v-if = "responseAvailable == true">
      <h2>Map of local area</h2>
      <img v-bind:src="mapURL">
      <br>
      <br>
      <br>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      responseAvailable: false,
      long: null,
      lat: null,
      isReady: false,
      mapURL: null,
    }
  },
  name: 'Map',
  props: ['zipcode'],
  watch: {
    zipcode: {
      handler() {
        console.log("everything is awesonme")
        this.getCoords();
      }
    },
    isReady: {
      handler() {
        console.log("ADAM")
        this.displayMap();
      }
    }
  },
  methods: {
    getCoords: function() {
      console.log("this is called in functino", this.zipcode)
      fetch('https://google-maps-geocoding.p.rapidapi.com/geocode/json?language=en&address='+ this.zipcode, {
	"method": "GET",
	"headers": {
		"x-rapidapi-host": "google-maps-geocoding.p.rapidapi.com",
		"x-rapidapi-key": "43aab3d406mshab354846eb51230p1e10f3jsn1f88db389414"
	}
})
.then(response => {
  console.log("first" , response);
  if (response) {
    return response.json()
  } else {
    alert ("Server returned " + response.status + " : " + response.statusText );
  }
})
.then(response => {
  console.log("final: ", response)
  this.long = response.results[0].geometry.location.lng;
  this.lat = response.results[0].geometry.location.lat
  this.isReady = !this.isReady;
  console.log("long: ", this.long);
  console.log("lat: ", this.lat);
})
.catch(err => {
	console.log(err);
});
    },
    displayMap() {
      this.responseAvailable = true;
      this.mapURL = 'https://maps.googleapis.com/maps/api/staticmap?center=' + this.lat + ',' + this.long + '&zoom=13&size=600x300&maptype=roadmap&markers=color:blue%7Clabel:S%7C40.702147,-74.015794&markers=color:green%7Clabel:G%7C40.711614,-74.012318&markers=color:red%7Clabel:C%7C40.718217,-73.998284&key=AIzaSyAGN_gNfw6U8jULAkuUSNGR7qlHKwAtyBk'

    }    
  }
  
}
</script>



<style scoped>

</style>