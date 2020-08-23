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
     
    }

  }
  
}
</script>

<style scoped>

</style>