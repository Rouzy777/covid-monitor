<template>
  <div class='d-flex min-vh-100' id="app">
      <div class="col-12 m-auto">
          <h1 class='mt-4'>COVID-19 Monitor</h1>
          <router-view/>
      </div>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Cabin:wght@400;700&display=swap');
#app {
  font-family: Cabin, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
