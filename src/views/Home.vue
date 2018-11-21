<template>
  <div class="home">
    <div v-for="place in places">
      <p>name: {{place.name}}</p>
      <p>address: {{place.address}}</p>
    </div>
    <p>name:<input type="text" v-model="newPlace.name"></p>
    <p>address:<input type="text" v-model="newPlace.address"></p>
    <button v-on:click="addPlace()">click here to add a place</button>
  </div>
</template>

<style>
</style>

<script>
// var axios = require('axios');
import axios from 'axios'

export default {
  data: function() {
    return {
      message: " to Vue.js!",
      places: [],
      newPlace: {name: "something", address: "4567890"}
    };
  },
  created: function() {
    // axios.get('http://localhost:3000/places').then(function(response) {
    axios.get('http://localhost:3000/places').then(response => {
      console.log(response.data);
      this.places = response.data
    });
  },
  methods: {
    addPlace: function() {
      console.log('i am in add place');
      var params = {
        name: this.newPlace.name,
        address: this.newPlace.address
      };
      axios.post('http://localhost:3000/places', params).then(response => {
        this.places.push(response.data);
      })
    }
  },
  computed: {}
};
</script>
