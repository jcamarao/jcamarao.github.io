<template>
  <Welcome/>
  <Navbar/>
  <MainPage/>
</template>

<script>
  import Welcome from './components/Welcome.vue'
  import Navbar from './components/Navbar.vue'
  import MainPage from './components/MainPage.vue'

  export default {
    name: 'App',
    components: {
      Welcome,
      Navbar,
      MainPage
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
