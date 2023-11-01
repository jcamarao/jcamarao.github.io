<template>
  <div id="mobile" v-if="isMobile()">
    <div id="mobile-navbar">
      <h1 class="mobile-nav-text">mobile</h1>
      <h1 class="mobile-nav-text">Lorem ipsum dolor sit amet</h1>
    </div>
  </div>
  <div id="desktop" v-else>
    <div id="desktop-navbar">
      <h1 class="desktop-nav-text">desktop</h1>
      <h1 class="desktop-nav-text">Lorem ipsum dolor sit amet</h1>
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
  div {
    margin-left: calc(-50vw + 50%);
  }
  #mobile-navbar {
    position: fixed;
    top: 0px;
    background-color: #131516;
    font-weight: bold;
    height: 8vh;
    width: 100vw;
    margin-left: calc(-50vw + 50%);
  }
  .mobile-nav-text{
    text-align: center;
    color: rgb(174, 194, 211);
    font-size: 3vw;
    font-family: 'Courier New', Courier, monospace;
    font-weight: bold;
  } 
  #desktop-navbar { 
    position: fixed;
    top: 0px;
    background-color: #131516;
    font-weight: bold;
    height: 10vh;
    width: 100vw;

  }
  .desktop-nav-text{
    text-align: center;
    top: 10px;
    color: rgb(174, 194, 211);
    font-size: 1.5vw;
    font-family: 'Courier New', Courier, monospace;
    font-weight: bold;
  }
</style>