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
      <!-- Large modal -->
<button class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">Create a New Event!</button>
<div></div>
<div>
  Search: <input type="text" v-model="searchFilter">
</div>

<div class="modal fade" tabindex="-1" role="dialog" id="exampleModal">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <form v-on:submit.prevent="submit()">
        <div class="form-group">
          <label>Event Title:</label>
          <input type="text" class="form-control" v-model="title">
        </div>
        <div class="form-group">
          <label>Event Description:</label>
          <input type="text" class="form-control" v-model="description">
        </div>
        <div class="form-group">
          <label>Event Address:</label>
          <input type="text" class="form-control" v-model="address">
        </div> 
        <div class="form-group">
          <label>Date of Event:</label>
          <input type="text" class="form-control" v-model="date">
        </div>  
        <div class="form-group">
          <label>Event Tags (Separated By Commas)</label>
          <input type="text" class="form-control" v-model="tag_names">
          <input type="button" class="btn btn-primary" value="Auto-Generate Tags" v-on:click="generateTags()">
        </div> 
        <div class="form-group">
          <label>Event Image URLs (Separated by Commas)</label>
          <input type="text" class="form-control" v-model="image_urls">
        </div>
        <input type="submit" class="btn btn-primary" value="Submit">
      </form>
    </div>
  </div>
</div>
    </div>
  </div>

  <div class="blog-cols-wrapper">
    <div class="container">
      <a v-bind:href=" '/#/events/show/' + event.id " class="post" v-for="event in filterBy(events, searchFilter, 'title', 'tags')">
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


<script>
/* global google, $ */

var axios = require("axios");
export default {
  data: function() {
    return {
      events: [],
      codedAddresses: [],
      map: null,
      title: "",
      description: "",
      address: "",
      formattedAddress: "henlo",
      date: "",
      tag_names: "",
      image_urls: "",
      searchFilter: "",
      errors: []
    };
  },
  created: function() {},
  mounted: function() {
    var styledMapType = new google.maps.StyledMapType([
      {
        featureType: "all",
        elementType: "geometry.fill",
        stylers: [
          {
            weight: "2.00"
          }
        ]
      },
      {
        featureType: "all",
        elementType: "geometry.stroke",
        stylers: [
          {
            color: "#9c9c9c"
          }
        ]
      },
      {
        featureType: "all",
        elementType: "labels.text",
        stylers: [
          {
            visibility: "on"
          }
        ]
      },
      {
        featureType: "landscape",
        elementType: "all",
        stylers: [
          {
            color: "#f2f2f2"
          }
        ]
      },
      {
        featureType: "landscape",
        elementType: "geometry.fill",
        stylers: [
          {
            color: "#ffffff"
          }
        ]
      },
      {
        featureType: "landscape.man_made",
        elementType: "geometry.fill",
        stylers: [
          {
            color: "#ffffff"
          }
        ]
      },
      {
        featureType: "poi",
        elementType: "all",
        stylers: [
          {
            visibility: "off"
          }
        ]
      },
      {
        featureType: "road",
        elementType: "all",
        stylers: [
          {
            saturation: -100
          },
          {
            lightness: 45
          }
        ]
      },
      {
        featureType: "road",
        elementType: "geometry.fill",
        stylers: [
          {
            color: "#eeeeee"
          }
        ]
      },
      {
        featureType: "road",
        elementType: "labels.text.fill",
        stylers: [
          {
            color: "#7b7b7b"
          }
        ]
      },
      {
        featureType: "road",
        elementType: "labels.text.stroke",
        stylers: [
          {
            color: "#ffffff"
          }
        ]
      },
      {
        featureType: "road.highway",
        elementType: "all",
        stylers: [
          {
            visibility: "simplified"
          }
        ]
      },
      {
        featureType: "road.arterial",
        elementType: "labels.icon",
        stylers: [
          {
            visibility: "off"
          }
        ]
      },
      {
        featureType: "transit",
        elementType: "all",
        stylers: [
          {
            visibility: "off"
          }
        ]
      },
      {
        featureType: "water",
        elementType: "all",
        stylers: [
          {
            color: "#46bcec"
          },
          {
            visibility: "on"
          }
        ]
      },
      {
        featureType: "water",
        elementType: "geometry.fill",
        stylers: [
          {
            color: "#c8d7d4"
          }
        ]
      },
      {
        featureType: "water",
        elementType: "labels.text.fill",
        stylers: [
          {
            color: "#070707"
          }
        ]
      },
      {
        featureType: "water",
        elementType: "labels.text.stroke",
        stylers: [
          {
            color: "#ffffff"
          }
        ]
      }
    ]);
    this.map = new google.maps.Map(document.getElementById("map"), {
      zoom: 3,
      center: { lat: 35.344, lng: 280.036 },
      mapTypeControlOptions: {
        mapTypeIds: ["styled_map"]
      }
    });
    this.map.mapTypes.set("styled_map", styledMapType);
    this.map.setMapTypeId("styled_map");
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
    },
    submit: function() {
      var geocoder = new google.maps.Geocoder();
      var that = this;
      var address = this.address;
      geocoder.geocode({ address: address }, function(results, status) {
        var addressResults = results[0].formatted_address;
        this.formattedAddress = addressResults;
        if (status === "OK") {
          that.formattedAddress = this.formattedAddress;
          console.log(this.formattedAddress);
          var params = {
            title: that.title,
            description: that.description,
            address: this.formattedAddress,
            date: that.date,
            tag_names: that.tag_names,
            image_urls: that.image_urls
          };
          console.log(params);
          axios
            .post("http://localhost:3000/api/events", params)
            .then(response => {
              console.log(response.data);
              that.events.push(response.data);
              that.codeAddresses();
              // this.$router.go();
              $("#exampleModal").modal("hide");
              $("body").removeClass("modal-open");
              $(".modal-backdrop").remove();
            })
            .catch(error => {
              this.errors = error.response.data.errors;
            });
        } else {
          alert(
            "Geocode was not successful for the following reason: " + status
          );
        }
      });
    },
    generateTags: function() {
      var params = {
        text: this.description
      };
      axios.post("http://localhost:3000/api/watson", params).then(response => {
        console.log(response);
        this.tag_names = response.data.keywords
          .map(elem => elem.text)
          .join(", ");
      });
    }
  },
  computed: {}
};
</script>
