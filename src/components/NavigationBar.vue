<template>
    <div id="nav-div">
        <nav class="navbar navbar-expand-lg navbar-static-top navbar-dark bg-dark">
            <a class="navbar-brand" href="#">Brittany Eccles</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item" :class="current == 'Home' ? 'active' : ''">
                        <a class="nav-link" href="#">Home</a>
                    </li>
                    <li class="nav-item" :class="current == 'Contact Us' ? 'active' : ''">
                        <a class="nav-link" href="#">Contact Us</a>
                    </li>
                </ul>
            </div>
        </nav>
    </div>
</template>

<script>
export default {
  name: 'App',
  props: {
    current: String,
  }
}
</script>

<style>
.navbar {
    position: absolute;
    top: 0px;
    width: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#welcome-img {
    width: 100%;
}
</style>