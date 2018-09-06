<template>
  <div class="user-events">
    <body class="timeline-page">
<!--     <div v-for="userEvent in userEvents"> 
      <h3> {{userEvent.title}} </h3>
      <p> {{userEvent.address}} </p>
      <p> {{userEvent.date}} </p>
    </div> -->
  </body>
  <div class="blog-timeline-header">
    <div class="container">
<!--       <div class="search">
        <form>
          <input type="text" class="form-control" placeholder="Search the blog...">
          <a href="#">
            <i class="ion-search"></i>
          </a>
        </form>
      </div> -->
      <h1>
          Events You're Attending
      </h1>
      <p class="slogan">
      </p>
    </div>
  </div>
    <div class="blog-timeline-wrapper">
      <div class="container" v-for="userEvent in userEvents">
        {{leftFlag()}}
        <div v-if="flag === false" class="post" >
          <div class="marker"></div>
          <div class="news">
            <section>
              <h3>
                  {{userEvent.title}}
              </h3>
              <p>
                {{userEvent.address}}
              </p>
              <p>
                {{userEvent.date}}
              </p>
              <a v-bind:href=" '/#/events/show/' + userEvent.id " class="read-more">
                Go to Event Page
              </a>
            </section>
            <footer class="clearfix">
              <span class="tag float-left">
                <span class="ion-ios-pricetag-outline"></span>
                <div v-for="tags in userEvent.tags">
                  {{tags.name}}
                </div>
              </span>
              <span class="date float-right">{{userEvent.date}}</span>
            </footer>
          </div>
        </div>
      <div v-else-if="flag === true" class="post left" >
          <div class="marker"></div>
          <div class="news">
            <div class="author">
            </div>
            <section>
              <h3>
                  {{userEvent.title}}
              </h3>
              <p>
                {{userEvent.address}}
              </p>
              <p>
                {{userEvent.date}}
              </p>
              <a v-bind:href=" '/#/events/show/' + userEvent.id " class="read-more">
                Go to Event Page
              </a>
            </section>
            <footer class="clearfix">
              <span class="tag float-left">
                <span class="ion-ios-pricetag-outline"></span>
                <div v-for="tags in userEvent.tags">
                  {{tags.name}}
                </div>
              </span>
              <span class="date float-right">{{userEvent.date}}</span>
            </footer>
          </div>
        </div>
       </div>
     </div>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      userEvents: [],
      flag: false
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/user-events").then(response => {
      this.userEvents = response.data;
      console.log(this.userEvents);
    });
  },
  methods: {
    leftFlag: function() {
      this.flag = !this.flag;
    }
  }
};
</script>
