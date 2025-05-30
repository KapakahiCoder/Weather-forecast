<template>
  <div id="app">
    <h1>Let's check out the weather forecast for your area</h1>
    <form id="form" ref="form" @submit.prevent>
      <b-input-group prepend="Area:">
        <div>
          <b-form-input class="form-control" v-model="code"></b-form-input>
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
      <h5>
        Please enter a zipcode and country or a description of the area you want
        to lookup.
      </h5>
      <h6>
        For example: "272-0022 Japan", "Tokyo Tower", or "San Jose, California"
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
      this.zipcode = postalCode;
      fetch(
        `https://maps.googleapis.com/maps/api/geocode/json?address=${encodeURIComponent(this.zipcode)}&key=${process.env.VUE_APP_GOOGLE_API_KEY}`,
        {
          method: "GET"
        }
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          } else {
            throw new Error(`HTTP error! status: ${response.status}`);
          }
        })
        .then((response) => {
          console.log(response, "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!");
          if (response.status == "ZERO_RESULTS") {
            alert(
              "Could you please enter more information? For example, '90210, USA' or 'Eiffel Tower'."
            );
            this.reloadPage();
          }
          this.$refs.form.reset();
          this.state = response.results[0].address_components[2].long_name;
          this.town = response.results[0].address_components[2].long_name;
          this.lon = response.results[0].geometry.location.lng;
          this.lat = response.results[0].geometry.location.lat;
        })
        .catch((err) => {
          console.error(err);
          alert("Error fetching location data. Please check your API key.");
        });
    },
    reloadPage() {
      window.location.reload();
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
