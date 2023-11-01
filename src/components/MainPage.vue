<template>
  <div id="mobile" v-if="isMobile()">
    <div id="mobile-navbar">
      <h1></h1>
    </div>
  </div>
  <div id="not-mobile" v-else>
    <div id="desktop-navbar">
      <h1></h1>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'MainPage',
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
#mobile {
  background-color: #131516;
}
#desktop {
  background-color: #131516;
}
</style>
