<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-light" role="navigation">
    <div class="container flex-lg-column">
      <a class="navbar-brand m-0 mt-lg-4" v-bind:href="'/#/'">
        <img src="images/logo-alt-b.png" class="d-none d-lg-inline mr-2 w-25" />
        Flock
      </a>
      <button class="navbar-toggler" data-toggle="collapse" data-target="#navbar-collapse">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse justify-content-center" id="navbar-collapse">
        <ul class="navbar-nav">
           <li class="nav-item">
            <a class="nav-link" href="#/">Home</a>
          </li>
          <li class="nav-item active dropdown">
            <a href="#" class="nav-link dropdown-toggle" data-toggle="dropdown">
              Your Events
              <i class="ion-chevron-down"></i>
            </a>
            <div class="dropdown-menu" role="menu">
              <a class="dropdown-item" href="#/attending">Attending</a>
            </div>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#/login">Login</a>
          </li>
           <li class="nav-item">
            <a class="nav-link" href="#/logout">Logout</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
    <router-view/>

  </div>
</template>

<style>
</style>
