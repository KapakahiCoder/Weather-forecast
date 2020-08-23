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
      <h3>Here is code: {{code}} </h3>
      <h3>Please enter valid Japanese postal code without any hypens or spaces</h3>
    </div>

  </div>

  
</template>

<script>
const postal_code = require('japan-postal-code');

export default {
  data() {
    return {
      code: null
    }
  },
  name: 'GetLocation',
  methods: {
    search: function() {
      console.log("Dadsdfasdf")
      postal_code.get('1000001', function(address) {
  console.log(address.prefecture); // => "東京都"
  console.log(address.city); // => "千代田区"
  console.log(address.area); // => "千代田"
  console.log(address.street); // => ""
});
    }

  }
  
}
</script>

<style scoped>

</style>