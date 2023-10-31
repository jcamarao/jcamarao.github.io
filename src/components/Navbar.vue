<template>
  <div id="mobile" v-if="isMobile()">
    <div id="mobile-navbar">
      <h1>mobile</h1>
    </div>
  </div>
  <div id="not-mobile" v-else>
    <div id="desktop-navbar">
      <h1>desktop</h1>
    </div>
  </div>
</template>
  
<script>    
  export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Navbar',
    props: {
      msg: String
    },
    // setup() runs prior to any html being loaded onto the page 
    setup() {
      const isMobile = () => navigator.userAgent.includes("Mobi") ? true : false
      return {isMobile}
    }
  }
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #terminal-bar {
    background-color: #ffffff;
    font-weight: bold;
    height: 10vh;
    width: 100%;
  }
  h1 {
    text-align: center;
    font-size: 3vw;
    font-family: 'Courier New', Courier, monospace;
    font-weight: bold;
  }
</style>