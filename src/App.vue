<template>
  <div class="filter">
    <div id="app">
      <router-view />
    </div>
  </div>  
</template>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.filter{
  height: 100vh;
  background:rgba(255,255,255, 0.5);
}
#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

body {
  background: url('https://qsnoticias.mx/wp-content/uploads/2019/05/301790.jpg');
  background-size: cover;
  background-position: center;
}
</style>
