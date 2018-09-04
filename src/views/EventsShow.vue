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
          <div v-if= "isAttending() === true">
            <a href="#" class="btn-get-tickets"  v-on:click="unattendEvent(), toggleAttending()">
              You Are Attending (Click to Un-RSVP)
            </a>
          </div>
          <div v-else= "isAttending() === false">
            <a href="#" class="btn-get-tickets"  v-on:click="attendEvent(), toggleAttending()">
              RSVP
            </a>
          </div>
          <p class="m-0">
            {{event.tags}}
          </p>
        </div>
      </div>
    </div>
    <div class="event-intro">
      <div class="container no-override">
        <div class="row">
          <div class="col-lg-7 order-lg-2">
            <div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
              <ol class="carousel-indicators">
                <!-- <li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li> -->
                <!-- <li data-target="#carouselExampleIndicators" data-slide-to="1"></li> -->
                <!-- <li data-target="#carouselExampleIndicators" data-slide-to="2"></li> -->
                <li v-for="(image, index) in images" data-target="#carouselExampleIndicators" v-bind:data-slide-to="index" v-bind:class="{active: index === 0}"></li>
              </ol>
              <div class="carousel-inner" role="listbox">
                <!-- <div class="carousel-item active">
                  <img class="d-block img-fluid" src="https://media.istockphoto.com/photos/blue-sky-and-white-clouds-background-picture-id825778252?k=6&m=825778252&s=612x612&w=0&h=VPlp3pwPLCK2fUhhLEO_p-hdQ7Pa1CVtLcAHZBLv4cM=" alt="First slide">
                  <div class="carousel-caption d-none d-md-block"></div>
                </div>
                <div class="carousel-item">
                  <img class="d-block img-fluid" src="https://media.istockphoto.com/photos/blue-sky-and-white-clouds-background-picture-id825778252?k=6&m=825778252&s=612x612&w=0&h=VPlp3pwPLCK2fUhhLEO_p-hdQ7Pa1CVtLcAHZBLv4cM=" alt="First slide">
                  <div class="carousel-caption d-none d-md-block"></div>
                </div> -->
                <div v-for="(image, index) in images" class="carousel-item" v-bind:class="{active: index === 0}">
                  <img class="d-block img-fluid" v-bind:src="image" alt="First slide">
                  <div class="carousel-caption d-none d-md-block">
                    <!-- <h2>{{ image }}</h2> -->
                  </div>
                </div>
              </div>
              <a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="sr-only">Previous</span>
              </a>
              <a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="sr-only">Next</span>
              </a>
            </div>
          </div>
          <div class="col-lg-5 order-lg-1">
            <h1 class="event-title">
              {{event.title}}
            </h1>
            <p>
              {{event.description}}
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
              {{formattedAddress}}
            </p>
            <p class="mt-4">
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
    <div class="event-tickets">
      <div class="container">
        <div class="row">
        </div>
        <div class="row">
          <div class="col-md-6">
            <div class="ticket">
              <p class="ticket-title">
                Comments
              </p>
              <hr>
              <h5>...</h5>
              <p>
                {{ event.comments }}
              </p>
            </div>
          </div>
          <div class="col-md-6">
            <div class="ticket">
              <p class="ticket-title">
                Attendees
              </p>
              <hr>
              <h5>...</h5>
              <p>
                {{ attendees }}
              </p>
            </div>
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

.carousel img {
  width: 100%;
}
</style>

<script>
/* global google */

var axios = require("axios");
export default {
  data: function() {
    return {
      event: [],
      attendees: [],
      images: [],
      codedAddress: [],
      formattedAddress: "",
      attending: undefined,
      currentUser: ""
    };
  },
  created: function() {},
  mounted: function() {
    this.map = new google.maps.Map(document.getElementById("map"), {
      zoom: 11
    });
    axios
      .get("http://localhost:3000/api/events/" + this.$route.params.id)
      .then(response => {
        this.event = response.data;
        this.currentUser = response.data.current_user;
        this.images = response.data.images;
        this.attendees = this.event.attendees;
        this.codeAddress();
      });
  },
  methods: {
    codeAddress: function() {
      var geocoder = new google.maps.Geocoder();
      var that = this;
      // var formattedAddress = this.longAddress;
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
          that.formattedAddress = longAddressResults;
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
    },
    attendEvent: function() {
      var params = {
        event_id: this.event.id
      };
      axios
        .post("http://localhost:3000/api/user-events", params)
        .then(response => {
          this.attendees.push(this.currentUser);
        });
    },
    unattendEvent: function() {
      axios
        .delete("http://localhost:3000/api/user-events/" + this.event.id)
        .then(response => {
          var index = this.attendees.indexOf(this.currentUser);
          this.attendees.splice(index, 1);
        });
    },
    isAttending: function() {
      if (this.attendees.includes(this.currentUser)) {
        this.attending = true;
        return true;
      } else {
        this.attending = false;
        return false;
      }
    },
    toggleAttending: function() {
      if (this.attending === false) {
        this.attending = true;
      } else {
        this.attending = false;
      }
    }
  },
  computed: {}
};
</script>
