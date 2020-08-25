<template>
  <div>
    <div v-if = "responseAvailable == true">
      <div>
        <b-table striped hover :items="items">
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
      items: [
        {date: null, weather: null, icon: null, minTemp: null, maxTemp: null},
        {date: null, weather: null, icon: null, minTemp: null, maxTemp: null},
        {date: null, weather: null, icon: null, minTemp: null, maxTemp: null}
      ],
    }
  },
  name: 'Weather',
  props: ['town', 'state'],
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
.then(response => {272
  this.items[0].date = (response.list[3].dt_txt).substring(0,10);
  this.items[0].weather = response.list[3].weather[0].main;
  this.items[0].icon = '<img src= "http://api.openweathermap.org/img/w/' + response.list[3].weather[0].icon + '.png">';
  this.items[0].minTemp = ((response.list[3].main.temp_min) - 273.15).toFixed(3);
  this.items[0].maxTemp = ((response.list[3].main.temp_max) - 273.15).toFixed(3);
  this.items[1].date = (response.list[11].dt_txt).substring(0,10);
  this.items[1].weather = response.list[11].weather[0].main;
  this.items[1].icon = '<img src= "http://api.openweathermap.org/img/w/' + response.list[11].weather[0].icon + '.png">';
  this.items[1].minTemp = ((response.list[11].main.temp_min) - 273.15).toFixed(3);
  this.items[1].maxTemp = ((response.list[11].main.temp_max) - 273.15).toFixed(3);
  this.items[2].date = (response.list[19].dt_txt).substring(0,10);
  this.items[2].weather = response.list[19].weather[0].main;
  this.items[2].icon = '<img src= "http://api.openweathermap.org/img/w/' + response.list[19].weather[0].icon + '.png">';
  this.items[2].minTemp = ((response.list[19].main.temp_min) - 273.15).toFixed(3);
  this.items[2].maxTemp = ((response.list[19].main.temp_max) - 273.15).toFixed(3);
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