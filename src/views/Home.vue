<template>
  <div class="home">
    <div id='map'></div>
    <!-- <div v-for="place in places">
      <p>name: {{place.name}}</p>
      <p>address: {{place.address}}</p>
    </div>
    <p>name:<input type="text" v-model="newPlace.name"></p>
    <p>address:<input type="text" v-model="newPlace.address"></p>
    <button v-on:click="addPlace()">click here to add a place</button> -->
  </div>
</template>

<style>
  body { margin:0; padding:0; }
  #map { position:absolute; top:0; bottom:0; width:100%; height: 400px; }
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
  mounted: function() {
    mapboxgl.accessToken = 'pk.eyJ1IjoiYnJpYW5yZ3J2ZiIsImEiOiJjanAzYWhhNGIwZnhnM2tvYnJoZWk1YzA0In0.DDR17tYl3BvJHR2FdON2sQ';
    var map = new mapboxgl.Map({
        container: 'map', // container id
        style: 'mapbox://styles/mapbox/streets-v9', // stylesheet location
        center: [-86.7816, 36.1627], // starting position [lng, lat]
        zoom: 9, // starting zoom
        pitch: 60
    });
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
