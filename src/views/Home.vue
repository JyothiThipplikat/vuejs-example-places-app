<template>
  <div class="home">
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <div>
      Name: <input v-model="newPlace.name">
      Address: <input v-model="newPlace.address">
      <button v-on:click="addPlace()">Add Place</button>
    </div>

    <div v-for="place in places"> {{places}}</div>
  </div>
</template>

<style>
</style>

<script>
  var axios = require('axios');
export default {
  data: function() {
    return {
      places: [],
      newPlace: {name: "", address:""},
      errors: []
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/places")
    .then(response => {
      this.places = response.data;
    });
  },
  methods: {
    addPlace: function() {
      this.errors = [];
      var params = {
                    name: this.newPlace.name,
                    address: this.newPlace.address
                    };
      axios
      .post("http://localhost:3000/api/places", params)
      .then(response => {
        this.places.push(response.data);
        this.newPlace = {name: "", address: ""};
      })
      .catch(error => {
        this.errors = error.response.data.errors;
      }
      });
  },
  computed: {}
};
</script>