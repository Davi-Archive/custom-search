<template>
  <nav class="app bg-center text-3xl">
    <router-link to="/">
      home
    </router-link> |
    <router-link to="/blogpost/688159">
      blogpost
    </router-link> |
    <router-link to="/aboutView">
      about
    </router-link>
  </nav>
  <div class="router">
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.pages {
  margin-top: 20px;
  padding: 3px;
  display: inline-block;
}

* {
  padding: 0;
  margin: 0;
}
</style>