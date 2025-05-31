<template>
  <div>
    <div v-if="error" class="error-message">
      <b-alert show variant="danger">{{ error }}</b-alert>
    </div>
    <div v-else-if="loading">
      <b-spinner label="Loading..."></b-spinner>
    </div>
    <div v-else-if="responseAvailable">
      <div>
        <hr />
        <h2>Here is the weather report for {{ state }}, {{ town }}:</h2>
        <hr />
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
import { BSpinner, BAlert } from 'bootstrap-vue'

export default {
  components: {
    BSpinner,
    BAlert
  },
  data() {
    return {
      responseAvailable: false,
      loading: false,
      error: null,
      fields: [
        { key: "date" },
        { key: "weather" },
        { key: "icon" },
        { key: "temp", label: "Temp (C°)" },
        { key: "humidity", label: "Humidity (%)" },
      ],
      items: [
        { date: null, weather: null, icon: null, temp: null, humidity: null },
        { date: null, weather: null, icon: null, temp: null, humidity: null },
        { date: null, weather: null, icon: null, temp: null, humidity: null },
      ],
    };
  },
  name: "Weather",
  props: {
    town: {
      type: String,
    },
    state: {
      type: String,
    },
    lon: {
      type: Number,
    },
    lat: {
      type: Number,
    },
  },
  // When the "lon" prop changes, getWeather will get the weather forecast for that area
  watch: {
    lon: {
      handler() {
        this.getWeather();
      },
    },
  },
  methods: {
    convertToJST(unixTime) {
      const jstTime = unixTime * 1000;
      const jstDate = new Date(jstTime);

      // Extract the date compenents from the JST date
      const daysOfTheWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
      const year = jstDate.getFullYear();
      const month = jstDate.getMonth() + 1;
      const day = jstDate.getDate();
      const dayOfWeek = daysOfTheWeek[jstDate.getDay()];

      // Return the date in "YYYY-MM-DD (day)" format
      return `${year}-${month
        .toString()
        .padStart(2, "0")}-${day.toString().padStart(2, "0")} (${dayOfWeek})`;
    },
    getWeather() {
      if (!this.lat || !this.lon) {
        this.error = 'Location coordinates are required';
        return;
      }

      this.loading = true;
      this.error = null;
      const apiKey = process.env.VUE_APP_OPEN_WEATHER_KEY;

      if (!apiKey) {
        this.error = 'API key is not configured';
        this.loading = false;
        return;
      }

      fetch(
        `https://api.openweathermap.org/data/3.0/onecall?lat=${this.lat}&lon=${this.lon}&exclude=minutely,hourly&appid=${apiKey}`,
        {
          method: "GET",
        }
      )
      .then((response) => {
        console.log(response, "AAAAAAA")
        if (!response.ok) {
          throw new Error(`Weather API error: ${response.status}`);
        }
        return response.json();
      })
      .then((response) => {
        if (!response.daily || response.daily.length < 4) {
          throw new Error('Invalid weather data received');
        }
        this.updateWeatherData(response);
      })
      .catch((error) => {
        console.error('Weather API Error:', error);
        this.error = `Failed to fetch weather data: ${error.message}`;
      })
      .finally(() => {
        this.loading = false;
      });
    },
    updateWeatherData(response) {
      const days = [1, 2, 3].map(index => ({
        date: this.convertToJST(response.daily[index].dt),
        weather: response.daily[index].weather[0].description,
        icon: `<img src="https://openweathermap.org/img/w/${response.daily[index].weather[0].icon}.png">`,
        temp: (response.daily[index].temp.day - 273.15).toFixed(1),
        humidity: response.daily[index].humidity
      }));

      this.items = days;
      this.responseAvailable = true;
    },
  },
};
</script>

<style scoped></style>
