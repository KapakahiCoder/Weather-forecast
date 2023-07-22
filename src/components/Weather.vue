<template>
  <div>
    <div v-if="responseAvailable == true">
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
export default {
  data() {
    return {
      responseAvailable: false,
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
      fetch(
        `https://api.openweathermap.org/data/3.0/onecall?lat=` +
          this.lat +
          `&lon=` +
          this.lon +
          `&appid=` +
          process.env.VUE_APP_OPEN_WEATHER_KEY,
        {
          method: "GET",
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
          const firstDay = this.convertToJST(response.daily[1].dt);
          const secondDay = this.convertToJST(response.daily[2].dt);
          const thirdDay = this.convertToJST(response.daily[3].dt);

          // Weather forecast for day 1

          this.items[0].date = firstDay;
          this.items[0].weather = response.daily[1].weather[0].description;
          this.items[0].icon =
            '<img src= "http://api.openweathermap.org/img/w/' +
            response.daily[1].weather[0].icon +
            '.png">';
          this.items[0].temp = (response.daily[1].temp.day - 273.15).toFixed(3);
          this.items[0].humidity = response.daily[1].humidity;

          // Weather forecast for day 2
          this.items[1].date = secondDay;
          this.items[1].weather = response.daily[2].weather[0].description;
          this.items[1].icon =
            '<img src= "http://api.openweathermap.org/img/w/' +
            response.daily[2].weather[0].icon +
            '.png">';
          this.items[1].temp = (response.daily[2].temp.day - 273.15).toFixed(3);
          this.items[1].humidity = response.daily[2].humidity;

          // Weather forecast for day 3
          this.items[2].date = thirdDay;
          this.items[2].weather = response.daily[3].weather[0].description;
          this.items[2].icon =
            '<img src= "http://api.openweathermap.org/img/w/' +
            response.daily[3].weather[0].icon +
            '.png">';
          this.items[2].temp = (response.daily[3].temp.day - 273.15).toFixed(3);
          this.items[2].humidity = response.daily[3].humidity;

          this.responseAvailable = true;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style scoped></style>
