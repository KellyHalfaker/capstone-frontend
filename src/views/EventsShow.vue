<!-- <template>
  <div class="events-show">
    <h1>Event</h1>
    <ul>
      <div v-for="event in events">
        <h3>{{event.name}}</h3>
        <img v-bind:src="event.images[0]" width="200">
        <h5>{{product.price}}</h5>
        <p>{{product.supplier}}</p>
        <p>{{product.description}}</p>
      </div>
    </ul>
  </div>
</template>

<style>
</style>

<script>
var axios = require("axios");
export default {
  data: function() {
    return {
      products: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/products").then(
      function(response) {
        this.products = response.data;
      }.bind(this)
    );
  },
  methods: {},
  computed: {}
};
</script> -->


<template>
  <div class="events-show">
  <div class="event-hero">
    <div class="container">
      <h2 class="customFadeInUp">
        {{event.title}}
      </h2>
    </div>
    <div class="tickets container">
      <div class="d-flex flex-column flex-md-row justify-content-between align-items-center">
        <p class="m-md-0">
          {{event.date}}
        </p>
        <a href="#" class="btn-get-tickets">
          Get tickets
        </a>
        <p class="m-0">
          {{formattedAddress}}
        </p>
      </div>
    </div>
  </div>
    <div class="event-intro">
    <div class="container no-override">
      <div class="row">
        <div class="col-lg-7 order-lg-2">
          <img src="images/unsplash/obed-hernandez-113514.jpg" class="img-fluid">
        </div>
        <div class="col-lg-5 order-lg-1">
          <h1 class="event-title">
            Get inspired by the knowledge of world's best speakers
          </h1>
          <p>
            Spacial is an advanced theme solution for desktop, tablet, and mobile devices. It works in all browsers and your clients can pay instantly, to improve their experience at all times anywhere they go. Pick the pages that describe your business the best and improve your users experience. Track your progress and learn along all the way.
          </p>
          <p>
            Focus on creating and growing your projects and websites, and we‘ll take care of spinning up new designs for your users and making sure they’re great.
          </p>
        </div>
      </div>
    </div>
  </div>
    <div class="event-location">
    <div class="container">
      <div class="row">
        <div class="col-12 text-center">
          <h1 class="event-title d-inline-block">Location</h1>
        </div>
      </div>
      <div class="row no-gutters address-container">
        <div class="col-md-4 address">
          <h5>Address</h5>
          <p class="mt-4">
            Main Principal Conference
          </p>
          <p>
            10th Street 12/3 4580 San Francisco, Ca.
          </p>
          <p class="mb-0">
            <i class="ion-android-call"></i>
            01-800 555-3312
          </p>
          <p>
            <i class="ion-ios-email-outline"></i>
            support@spacialconf.com
          </p>
          <a class="social-link" href="#">
            <i class="ion-social-twitter"></i>
          </a>
          <a class="social-link" href="#">
            <i class="ion-social-facebook"></i>
          </a>
        </div>
        <div class="col-md-8">
          <div id="map"></div>
        </div>
      </div>
    </div>
  </div>
 </div>
</template>

<style>
#map {
  height: 300px;
  border: 300px;
}
</style>

<script>
/* global google */

var axios = require("axios");
export default {
  data: function() {
    return {
      event: [],
      codedAddress: [],
      formattedAddress: ""
    };
  },
  created: function() {},
  mounted: function() {
    console.log(this.formattedAddress);
    this.map = new google.maps.Map(document.getElementById("map"), {
      zoom: 11
    });
    axios
      .get("http://localhost:3000/api/events/" + this.$route.params.id)
      .then(response => {
        this.event = response.data;
        this.codeAddress();
      });
  },
  methods: {
    codeAddress: function() {
      var geocoder = new google.maps.Geocoder();
      var that = this;
      var formattedAddress = this.longAddress;
      var codedAddress = this.codedAddress;
      var event = this.event;
      var address = event.address;
      geocoder.geocode({ address: address }, function(results, status) {
        var latlng = {
          lat: results[0].geometry.location.lat(),
          lng: results[0].geometry.location.lng()
        };
        var longAddressResults = results[0].formatted_address;
        if (status === "OK") {
          formattedAddress = longAddressResults;
          codedAddress.push(latlng);
          that.createMarker(event, latlng);
          that.map.setCenter(latlng);
        } else {
          alert(
            "Geocode was not successful for the following reason: " + status
          ).bind(this);
        }
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
