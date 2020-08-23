<template>
  <div id="location">
    <h1>Let's checkout the weather forecast for your area</h1>
    <form @submit.prevent>
      <label>Search area: </label>
      <input type="text" v-model="code" />
      <p> 
        <button @click="search">Search</button> 
      </p>
    </form>
    <div>
      <h3>Here is code: {{result}} </h3>
      <h3>Please enter valid Japanese postal code without any hypens or spaces</h3>
    </div>

  </div>

  
</template>

<script>
export default {
  data() {
    return {
      code: null,
      responseAvailable: false,
      state: null,
      town: null,
    }
  },
  name: 'GetLocation',
  methods: {
    search: function() {
      console.log("Dadsdfasdf")
  this.responseAvailable = false;    
      fetch("https://postcodejp-api.p.rapidapi.com/postcodes?postcode=1000001", {
	"method": "GET",
	"headers": {
		"x-rapidapi-host": "postcodejp-api.p.rapidapi.com",
		"x-rapidapi-key": "43aab3d406mshab354846eb51230p1e10f3jsn1f88db389414"
	}
})
.then(response => {
    if (response.ok) {
      return response.json()
    } else {
      alert ("Server returned " + response.status + " : " + response.statusText );
    }
})
.then(response => {
  console.log("location object: ", response.data[0]);
  this.state = response.data[0].state;
  this.town = response.data[0].town;
  this.responseAvailable = true;
  console.log("town: ", this.result)
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