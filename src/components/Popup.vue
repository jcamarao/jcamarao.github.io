<template>
    <div id="mobile" v-if="isMobile()">
    </div>
    <div id="desktop" v-else>
    </div>
  </template>
  
  <script>
    export default {
      name: 'Popup',
      props: {
        msg: String
      },
      // setup() runs prior to any html being loaded onto the page 
      setup() {
        const isMobile = () => navigator.userAgent.includes("Mobi") ? true : false
        return { isMobile }
      }
    }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  #mobile {
    background-color: blue;
    /* background-color: #131516; */
    border-radius: 10px;
    position: absolute;
    height: 80vh;
    width: 80vw;
    left: 10vw;
    top: 5vh;
  }
  #desktop {
    /* background-color: #131516; */
    background-color: blue;
    border-radius: 10px;
    position: absolute;
    height: 80vh;
    width: 80vw;
    left: 10vw;
    top: 10vh;
  }
  </style>
  