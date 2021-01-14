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
  props: {
    zipcode: {
      type: String
    },
  },
  // When zipcode prop is updated, the getCoords function will trigger and get the long and lat coordinates
  watch: {
    zipcode: {
      handler() {
        this.getCoords();
      }
    },
  // Once the coordinates are returned, displayMap will run and get the map of the area  
    isReady: {
      handler() {
        this.displayMap();
      }
    }
  },
  methods: {
    getCoords: function() {
      fetch('https://google-maps-geocoding.p.rapidapi.com/geocode/json?language=en&address='+ this.zipcode, {
	"method": "GET",
	"headers": {
		"x-rapidapi-host": "google-maps-geocoding.p.rapidapi.com",
		"x-rapidapi-key": process.env.VUE_APP_RAPIDAPI_KEY
	}
})
.then(response => {
  if (response) {
    return response.json()
  } else {
    alert ("Server returned " + response.status + " : " + response.statusText );
  }
})
.then(response => {
  this.long = response.results[0].geometry.location.lng;
  this.lat = response.results[0].geometry.location.lat

  // This will toogle isReady and the change of state means the coordinates are returned
  // and this will trigger the displayMap function
  this.isReady = !this.isReady;
})
.catch(err => {
	console.log(err);
});
    },
    displayMap() {
      const googleToken = process.env.VUE_APP_GOOGLE_KEY;
      this.responseAvailable = true;
      this.mapURL = `https://maps.googleapis.com/maps/api/staticmap?center=` + this.lat + `,` + this.long + `&zoom=13&size=600x300&maptype=roadmap&markers=color:blue%7Clabel:S%7C40.702147,-74.015794&markers=color:green%7Clabel:G%7C40.711614,-74.012318&markers=color:red%7Clabel:C%7C40.718217,-73.998284&key=${googleToken}`
    }    
  }
}
</script>

<style scoped>

</style>