<template>
  <div id="app">
    <h1>Let's check out the weather forecast for your area</h1>
    <form id="form" @submit.prevent>
      <b-input-group prepend="Area:">
        <div>
          <b-form-input
            class="form-control"
            @keyup.enter="getLocation(code)"
            v-model="code"
          ></b-form-input>
        </div>
        <b-input-group-append>
          <b-button @click="getLocation(code)" variant="info" type="submit"
            >Search</b-button
          >
        </b-input-group-append>
      </b-input-group>
      <br />
    </form>

    <div>
      <h6>
        Please enter a valid Japanese postal code without any hypens or spaces
      </h6>
    </div>
    <Weather v-bind:town="town" :state="state" :lon="lon" :lat="lat" />
    <Map v-bind:lon="lon" :lat="lat" />
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";
import Map from "./components/Map.vue";

export default {
  data() {
    return {
      code: null,
      zipcode: null,
      responseAvailable: false,
      state: null,
      town: null,
      lon: null,
      lat: null,
    };
  },
  name: "App",
  components: {
    Weather,
    Map,
  },
  methods: {
    // Get the state, town and coordinates from user given zipcode
    getLocation: function(postalCode) {
      if (postalCode.length !== 7 || isNaN(postalCode) === true) {
        alert("Please enter a valid 7 digit Japanese postal code");
      }
      this.zipcode = postalCode;
      fetch(
        "https://google-maps-geocoding.p.rapidapi.com/geocode/json?language=en&address=" +
          this.zipcode,
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "google-maps-geocoding.p.rapidapi.com",
            "x-rapidapi-key": process.env.VUE_APP_RAPIDAPI_KEY,
          },
        }
      )
        .then((response) => {
          if (response) {
            return response.json();
          } else {
            alert(
              "Server returned " + response.status + " : " + response.statusText
            );
          }
        })
        .then((response) => {
          this.state = response.results[0].address_components[3].long_name;
          this.town = response.results[0].address_components[2].long_name;
          this.lon = response.results[0].geometry.location.lng;
          this.lat = response.results[0].geometry.location.lat;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
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

#form {
  margin-top: 30px;
  margin-left: 36%;
}
</style>
