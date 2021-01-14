<template>
  <div>
    <div v-if = "responseAvailable == true">
      <div>
        <b-table striped hover :items="items" :fields="fields">
          <template v-slot:cell(icon)="data">
            <span v-html="data.value"></span>
          </template>
        </b-table>
      </div>
    </div> 
  </div>
</template>


<script>
export default {
  data() {
    return {
      responseAvailable: false,
      fields: [ 
        { key: 'date'},
        { key: 'weather'},
        { key: 'icon'},
        { key: 'temp', label: 'Temp (C°)'},
        { key: 'humidity', label: 'Humidity (%)'}],
      items: [
        {date: null, weather: null, icon: null, temp: null, humidity: null},
        {date: null, weather: null, icon: null, temp: null, humidity: null},
        {date: null, weather: null, icon: null, temp: null, humidity: null}
      ],
    }
  },
  name: 'Weather',
  props: {
    town: String,
    state: String
  },
  // When the "state" prop changes, getWeather() will run
  watch: {
    state: {
      handler() {
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
    "x-rapidapi-key": process.env.VUE_APP_RAPIDAPI_KEY
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
  console.log(response)
  // Weather forecast for day 1
  this.items[0].date = (response.list[3].dt_txt).substring(0,10);
  this.items[0].weather = response.list[3].weather[0].main;
  this.items[0].icon = '<img src= "http://api.openweathermap.org/img/w/' + response.list[3].weather[0].icon + '.png">';
  this.items[0].temp = ((response.list[3].main.temp) - 273.15).toFixed(3);
  this.items[0].humidity = response.list[3].main.humidity;

  // Weather forecast for day 2
  this.items[1].date = (response.list[11].dt_txt).substring(0,10);
  this.items[1].weather = response.list[11].weather[0].main;
  this.items[1].icon = '<img src= "http://api.openweathermap.org/img/w/' + response.list[11].weather[0].icon + '.png">';
  this.items[1].temp = ((response.list[11].main.temp) - 273.15).toFixed(3);
  this.items[1].humidity = response.list[11].main.humidity;

  // Weather forecast for day 3
  this.items[2].date = (response.list[19].dt_txt).substring(0,10);
  this.items[2].weather = response.list[19].weather[0].main;
  this.items[2].icon = '<img src= "http://api.openweathermap.org/img/w/' + response.list[19].weather[0].icon + '.png">';
  this.items[2].temp = ((response.list[19].main.temp) - 273.15).toFixed(3);
  this.items[2].humidity = response.list[19].main.humidity;

  this.responseAvailable = true;
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