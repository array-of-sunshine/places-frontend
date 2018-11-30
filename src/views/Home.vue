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

  #marker {
      background-image: url('https://www.mapbox.com/mapbox-gl-js/assets/washington-monument.jpg');
      background-size: cover;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      cursor: pointer;
  }

  .mapboxgl-popup {
      max-width: 200px;
  }
</style>

<script>
// var axios = require('axios');
import axios from 'axios'

export default {
  data: function() {
    return {
      message: " to Vue.js!",
      places: [],
      newPlace: {name: "something", address: "4567890"},
      buildings: [
      { lng: -87.6322, lat: 41.8780, text: "Chicago Board of Trade"},
      { lng: -87.6237, lat: 41.8904, text: "Tribune Tower"},
      { lng: -87.6359, lat: 41.8789, text: "Sears Tower"}
    ]
    };
  },
  mounted: function() {
    var monument = [-77.0353, 38.8895];
    mapboxgl.accessToken = 'pk.eyJ1IjoiYnJpYW5yZ3J2ZiIsImEiOiJjanAzYWhhNGIwZnhnM2tvYnJoZWk1YzA0In0.DDR17tYl3BvJHR2FdON2sQ';
    var map = new mapboxgl.Map({
        container: 'map', // container id
        style: 'mapbox://styles/mapbox/streets-v9', // stylesheet location
        center: monument, // starting position [lng, lat]
        zoom: 9 // starting zoom

    });

    // var buildings = [[-87.6322, 41.8780, "Chicago Board of Trade"], [-87.6237, 41.8904, "Tribune Tower"], [-87.6359, 41.8789, "Sears Tower"]]




    // ||
    this.buildings.forEach(function(building) {
      var popup = new mapboxgl.Popup({ offset: 25 })
      // .setText(building[2]);
      .setText(building.text);

      // create DOM element for the marker


      // create the marker
      new mapboxgl.Marker()
          // .setLngLat([building[0], building[1]])
          .setLngLat([building.lng, building.lat])
          .setPopup(popup) // sets a popup on this marker
          .addTo(map);
    })
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
