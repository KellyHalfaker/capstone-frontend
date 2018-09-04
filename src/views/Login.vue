<template>
  <div class = "login">
    <head>
      <meta charset="utf-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Spacial - Sign in</title>
      <meta name="description" content="">
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    </head>
    <body class="signin-page">
      <div class="wrapper">
        <form v-on:submit.prevent="submit()">
          <ul>
            <li class="text-danger" v-for="error in errors">{{ error }}</li>
          </ul>
          <div class="form-field">
            <input type="email" placeholder="Email" v-model="email">
          </div>
          <div class="form-field">
            <input type="password" placeholder="Password" v-model="password">
          </div>
          <div class="form-action">
            <input type="submit" class="btn-shadow btn-shadow-primary" value="Log in">
          </div>
          <div class="form-bottom">
            Don't have an account yet?
            <a href="sign-up.html">Sign up</a>
          </div>
        </form>
      </div>
    </body>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      email: "",
      password: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        email: this.email,
        password: this.password
      };
      axios
        .post("http://localhost:3000/api/sessions", params)
        .then(response => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$router.push("/");
        })
        .catch(error => {
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    }
  }
};
</script>
