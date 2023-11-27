<template>
  <!-- YOU NEED TO SPECIFY THIS FOR SAFARI FOR THEIR NEW THEME-COLOR FEATURE OR YOU WILL CRY -->
  <meta name="theme-color" media="(prefers-color-scheme: dark)" content="#0e181f" />
  <Welcome @showPopup="imageClicked"/>
  <Popup v-if="showModal == true"></Popup>
</template>

<script>
  import { ref } from 'vue'
  import Welcome from './components/Welcome.vue'
  import Popup from './components/Popup.vue'

  export default {
    name: 'App',
    components: {
      Welcome,
      Popup
    },
    setup() {
      let showModal = ref(false)
      function imageClicked(value) {
        showModal.value = value
        console.log(showModal.value)
      }

      return {imageClicked, showModal }
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
  }
</style>
