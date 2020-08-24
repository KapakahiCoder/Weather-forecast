<template>
  <div>
    <h2>weather</h2>
    <h3>Does this work {{town}} {{state}} </h3>
  </div>
</template>


<script>
export default {
  data() {
    return {
   
    }
  },
  name: 'Weather',
  props: ['town', 'state'],
  watch: {
    state: {
      handler() {
        console.log("TRIGGGEERRRRRRR");
        this.getWeather();
        
      }
    }
  },
  methods: {
    getWeather() {
      fetch("https://community-open-weather-map.p.rapidapi.com/forecast?q=hawaii", {
	"method": "GET",
	"headers": {
		"x-rapidapi-host": "community-open-weather-map.p.rapidapi.com",
		"x-rapidapi-key": "43aab3d406mshab354846eb51230p1e10f3jsn1f88db389414"
	}
})
.then(response => {
  if (response) {
    return response.json();
  } else {
    alert ("Server returned " + response.status + " : " + response.statusText );
  }
})
.then(response => {
  console.log("weather::::" , response  )
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