<template>
  <div id="app">
    <h1>Let's check out the weather forecast for your area</h1>
    <form @submit.prevent>
      <label>Search area: </label>
      <input type="text" v-model="code" />
      <p> 
        <button @click="search(code)">Search</button> 
      </p>
    </form>
    <div>
      <h5>Please enter valid Japanese postal code without any hypens or spaces</h5>
    </div>
    <div v-if = "responseAvailable == true"> 
        <hr>
          <h2>Here is the weather report for {{state}}, {{town}}: </h2>
        <hr> 
    </div>
    <Weather v-bind:town="town" :state="state"/>
    <Map v-bind:zipcode="zipcode" />
  </div>
</template>

<script>
import Weather from './components/Weather.vue'
import Map from './components/Map.vue'

export default {
  data() {
    return {
      code: null,
      zipcode: null,
      responseAvailable: false,
      state: null,
      town: null,
    }
  },
  name: 'App',
  components: {
    Weather,
    Map
  },
  methods: {
    search: function(postalCode) {
      this.zipcode = postalCode;
  this.responseAvailable = false;    
      fetch('https://postcodejp-api.p.rapidapi.com/postcodes?postcode=' + postalCode, {
	"method": "GET",
	"headers": {
		"x-rapidapi-host": "postcodejp-api.p.rapidapi.com",
		"x-rapidapi-key": "43aab3d406mshab354846eb51230p1e10f3jsn1f88db389414"
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
  this.state = response.data[0].state;
  this.town = response.data[0].town;
  this.responseAvailable = true;
})
.catch(err => {
	console.log(err);
});
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
