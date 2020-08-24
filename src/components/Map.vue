<template>
  <div>
    <h2>MAP</h2>
    <h3>  {{town}}   </h3>
     <p> 
        <button @click="getCoords">Search</button> 
      </p>
  </div>
</template>


<script>
export default {
  data() {
    return {
      long: null,
      lat: null
    }
  },
  name: 'Map',
  props: ['town'],
  methods: {
    getCoords: function() {
      fetch('https://google-maps-geocoding.p.rapidapi.com/geocode/json?language=en&address=2720022', {
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
  console.log("long: ", this.long);
  console.log("lat: ", this.lat);
})
.catch(err => {
	console.log(err);
});
    }
  }
  
}
</script>



<style scoped>

</style>