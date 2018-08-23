<template>
  <div class="home">
    <div id="map"></div>
<!--     <button v-on:click="codeAddresses()">Click</button>
    <h1>Events</h1>
    <div v-for="event in events">
      <h2> {{event.title}}</h2>
    </div> -->
      <div class="blog-cols-header">
    <div class="container">
      <h1>
        Get the latest thoughts & news
      </h1>
      <p>
        Latest updates, projects and interesting news from our company
      </p>
    </div>
  </div>

  <div class="blog-cols-wrapper">
    <div class="container">
      <a href="blog-post.html" class="post" v-for="event in events">
        <div class="post-wrapper">
          <div class="post-bg" :style="{ backgroundImage: 'url(' + event.images[0] + ')' }">
          <!-- <div class="post-bg" :style="`background-image: url('${event.images[0]}');`"> -->
          </div>
          <div class="post-intro">
            <div class="post-title">
              {{ event.title }}
            </div>
            <div class="post-min-read">
              {{event.tags.join(", ")}}
            </div>
            <p class="post-description">
              {{event.address}}
            </p>
          </div>
        </div>
      </a>
    </div>
  </div>
<!-- <div class="post-bg" style="background-image: url('images/unsplash/photo-1466854076813-4aa9ac0fc347.jpg');"> -->
  <nav class="blog-pager">
    <ul class="pagination justify-content-center">
      <li class="page-item">
        <a class="page-link" href="#">&larr; Previous</a>
      </li>
      <li class="page-item active">
        <a class="page-link" href="#">1</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">2</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">3</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">4</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">Next &rarr;</a>
      </li>
    </ul>
  </nav>
  </div>
</template>
<style>
/* Always set the map height explicitly to define the size of the div
   * element that contains the map. */
#map {
  height: 300px;
  border: 300px;
}
#searchbox {
  height: 100px;
}
</style>

#searchbox {
height: 100px
}

<script>
/* global google */

var axios = require("axios");
export default {
  data: function() {
    return {
      events: [],
      codedAddresses: [],
      map: null
    };
  },
  created: function() {},
  mounted: function() {
    this.map = new google.maps.Map(document.getElementById("map"), {
      zoom: 3,
      center: myLatLng
    });
    axios.get("http://localhost:3000/api/events").then(response => {
      this.events = response.data;
      this.codeAddresses();
    });
  },
  methods: {
    codeAddresses: function() {
      var geocoder = new google.maps.Geocoder();
      var that = this;
      var codedAddresses = this.codedAddresses;
      this.events.forEach(function(event) {
        var address = event.address;
        geocoder.geocode({ address: address }, function(results, status) {
          var latlng = {
            lat: results[0].geometry.location.lat(),
            lng: results[0].geometry.location.lng()
          };
          if (status === "OK") {
            codedAddresses.push(latlng);
            console.log(codedAddresses);
            that.createMarker(event, latlng);
          } else {
            alert(
              "Geocode was not successful for the following reason: " + status
            ).bind(this);
          }
        });
      });
    },
    createMarker: function(event, latlng) {
      var marker = new google.maps.Marker({
        position: latlng,
        map: this.map,
        title: event.title
      });
    }
  },
  computed: {}
};
</script>
