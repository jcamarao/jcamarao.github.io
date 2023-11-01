<template>
    <div id="mobile" v-if="isMobile()">
        <div id="mobile-welcome">
            <h1 id="mobile-welcome-text">test</h1>
        </div>
    </div>
    <div id="not-mobile" v-else>
        <div id="desktop-welcome">
            <div data-text></div>
            <span class="desktop-welcome-text">Hi, I'm Jake.</span>
            <span class="desktop-welcome-text">Welcome to my portfolio.</span>
            <span class="desktop-welcome-text"></span>
        </div>
    </div>
</template>
  
<script>
    export default {
      // eslint-disable-next-line vue/multi-word-component-names
      name: 'Welcome',
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
    #mobile-welcome {
        position: fixed;
        top: 0;
        background-color: #131516;
        height: 100%;
        width: 100%;
        z-index: 99;
    }
    #mobile-welcome-text {
        position: fixed;
        top: 40%;
        left: 50%;
        text-align: center;
    }
    #desktop-welcome {
        position: fixed;
        top: 0;
        background-color: #131516;
        height: 100%;
        width: 100%;
        z-index: 99;
    }
    .desktop-welcome-text {
        display: flex;
        position: fixed;
        margin-left: auto;
        margin-right: auto;
        top: 35%;
        left: 45%;
        text-align: center;
    }
</style>
  