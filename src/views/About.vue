<template>
  <div class="about">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <router-link to="/write">Write</router-link>
    </div>
    <h1>This is an about page</h1>
  </div>
</template>
<style scoped>
#nav {
  padding: 30px;
  z-index: 1;
  position: absolute;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
