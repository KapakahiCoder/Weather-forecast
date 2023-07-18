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
      const googleToken = process.env.VUE_APP_GOOGLE_KEY;
      // Displays map only if responseAvailable is true
      this.responseAvailable = true;
      this.mapURL =
        `https://maps.googleapis.com/maps/api/staticmap?center=` +
        this.lat +
        `,` +
        this.lon +
        `&zoom=13&size=600x300&maptype=roadmap&markers=color:blue%7Clabel:S%7C40.702147,-74.015794&markers=color:green%7Clabel:G%7C40.711614,-74.012318&markers=color:red%7Clabel:C%7C40.718217,-73.998284&key=${googleToken}`;
    },
  },
};
</script>

<style scoped></style>
