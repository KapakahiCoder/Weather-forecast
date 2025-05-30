<template>
  <div>
    <div v-if="responseAvailable == true">
      <h2>Map of local area</h2>
      <img v-bind:src="mapURL" />
      <br />
      <br />
      <br />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      responseAvailable: false,
      mapURL: null,
    };
  },
  name: "Map",
  props: {
    lon: Number,
    lat: Number,
  },
  watch: {
    // When "lon" prop is updated returned, displayMap will run and get the map of the area
    lon: {
      handler() {
        this.displayMap();
      },
    },
  },
  methods: {
    displayMap() {
      if (!this.lat || !this.lon) return;

      const googleToken = process.env.VUE_APP_GOOGLE_API_KEY;
      // Displays map only if responseAvailable is true
      this.responseAvailable = true;
      
      this.mapURL = `https://maps.googleapis.com/maps/api/staticmap?`
        + `center=${this.lat},${this.lon}`
        + `&zoom=13`
        + `&size=600x300`
        + `&maptype=roadmap`
        + `&markers=color:red%7C${this.lat},${this.lon}`
        + `&key=${googleToken}`;
    },
  },
};
</script>

<style scoped></style>
