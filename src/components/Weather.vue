<template>
  <div>
    <h2>weather</h2>
    <h3>Does this work {{town}} {{state}} </h3>
    <div v-if = "responseAvailable == true">
      <p>Yo, the weather is {{todayWeather}}</p>
      <img v-bind:src="iconURL">
    </div> 
  </div>
</template>


<script>
export default {
  data() {
    return {
      responseAvailable: false,
      today: null, 
      todayWeather: null,
      todayIcon: null,
      todayMinTemp: null,
      todayMaxTemp: null,
      tomorrow: null, 
      tomorrowWeather: null,
      tomorrowIcon: null,
      tomorrowMinTemp: null,
      tomorrowMaxTemp: null,
      dayAfter: null, 
      dayAfterWeather: null,
      dayAfterIcon: null,
      dayAfterMinTemp: null,
      dayAfterMaxTemp: null,
      iconURL: null,
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
      fetch("https://community-open-weather-map.p.rapidapi.com/forecast?q="+this.town+","+this.state, {
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
  this.today = (response.list[3].dt_txt).substring(0,10);
  this.todayWeather = response.list[3].weather[0].main;
  this.todayIcon = response.list[3].weather[0].icon;
  this.todayMinTemp = ((response.list[3].main.temp_min) - 273.15).toFixed(3);
  this.todayMaxTemp = ((response.list[3].main.temp_max) - 273.15).toFixed(3);
  this.iconURL = 'http://api.openweathermap.org/img/w/' + this.todayIcon + '.png'
  this.responseAvailable = true;
  console.log("date: ", this.today);
  console.log("weather: ", this.todayWeather);
  console.log("icon: ", this.todayIcon);
  console.log("min temp: ", this.todayMinTemp);
  console.log("max temp: ", this.todayMaxTemp);
  console.log(typeof this.todayMinTemp)
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