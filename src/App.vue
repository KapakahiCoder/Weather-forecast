<template>
  <div id="app">
    <h1>Let's check out the weather forecast for your area</h1>
    <form id="form" @submit.prevent>
      <b-input-group prepend="Postal Code:">
        <div>
          <b-form-input class="form-control" @keyup.enter="search(code)" v-model="code"></b-form-input>
        </div>
        <b-input-group-append>
          <b-button @click="search(code)" variant="info">Search</b-button>
        </b-input-group-append>
      </b-input-group>
      <br />
    </form>

    <div>
      <h6>Please enter a valid Japanese postal code without any hypens or spaces</h6>
    </div>
    <div v-if="responseAvailable == true">
      <hr />
      <h2>Here is the weather report for {{state}}, {{town}}:</h2>
      <hr />
    </div>
    <Weather v-bind:town="town" :state="state" />
    <Map v-bind:zipcode="zipcode" />
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
    };
  },
  name: "App",
  components: {
    Weather,
    Map,
  },
  methods: {
    // Get the state and city name from user given zipcode
    // State and city name is needed to get the weather forecast
    search: function (postalCode) {
      if (postalCode.length !== 7 || isNaN(postalCode) === true) {
        alert("Please enter a valid 7 digit Japanese postal code");
      }
      this.zipcode = postalCode;
      this.responseAvailable = false;
      fetch(
        "https://postcodejp-api.p.rapidapi.com/postcodes?postcode=" +
          postalCode,
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "postcodejp-api.p.rapidapi.com",
            "x-rapidapi-key":
              process.env.VUE_APP_RAPIDAPI_KEY,
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
          this.state = response.data[0].state;
          this.town = response.data[0].town;
          this.responseAvailable = true;
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
