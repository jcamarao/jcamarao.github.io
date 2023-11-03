<template>
    <!-- YOU NEED TO SPECIFY THIS FOR SAFARI FOR THEIR NEW THEME-COLOR FEATURE OR YOU WILL CRY -->
    <meta name="theme-color" media="(prefers-color-scheme: dark)" content="#0e181f" />
    <div id="mobile" v-if="isMobile()">
        <div id="mobile-welcome">
            <!-- <div v-if="welcomeFinished"><img id="mobile-pic" :src="require(image)" /></div> -->
            <span class="mobile-welcome-text">{{ welcomeMessage }}</span>
            <span class="mobile-cursor">|</span>
            <div class="mobile-wrapper">
                <span id="mobile-transition-trigger-left" v-if="pictureVisible">{{ revealLeft }}</span>
                <span id="mobile-transition-trigger" v-if="pictureVisible">{{ reveal }}</span>
                <span id="mobile-transition-trigger-right" v-if="pictureVisible">{{ revealRight }}</span>
            </div>
        </div>
    </div>
    <div id="not-mobile" v-else>
        <div id="desktop-welcome">
            <!-- <div v-if="welcomeFinished"><img id="desktop-pic" :src="require(image)" /></div> -->
            <span class="desktop-welcome-text">{{ welcomeMessage }}</span>
            <span class="desktop-cursor">|</span>
            <div class="desktop-wrapper">
                <span id="desktop-transition-trigger-left" v-if="pictureVisible">{{ revealLeft }}</span>
                <span id="desktop-transition-trigger" v-if="pictureVisible">{{ reveal }}</span>
                <span id="desktop-transition-trigger-right" v-if="pictureVisible">{{ revealRight }}</span>
            </div>
        </div>
    </div>
</template>
  
<script>
    import { ref, onMounted } from 'vue'
    // import image from '../assets/self.png'

    export default {
        name: "Welcome",
        // setup() runs prior to any html being loaded onto the page 
        setup() {
            // mobile detection
            const isMobile = () => navigator.userAgent.includes("Mobi") ? true : false

            // variables for the type effects
            const reveal = ref("Learn more about me.")
            const revealLeft = ref("{")
            const revealRight = ref("}")
            const messages = ref(["Hey! Nice to meet you. :)", "I'm Jake!"])
            const typingSpeed = ref(100)
            const erasingSpeed = ref(75)
            const delayTimer = ref(1000)
            let welcomeMessage = ref("")
            let typing = ref(false)
            let welcomeFinished = ref(false)
            let pictureVisible = ref(false)
            let arrayPos = ref(0)
            let stringPos = ref(0)
            
            /* 
            * method for controlling the typing effect
            * welcomeMessage: array holding the strings to print
            * arrayPos: current position of the array
            * stringPos: current position of the string in welcomeMessage
            * typing: boolean to control whether or not we are still creating welcomeMessage
            * 
            * Slowly create welcomeMessage until filled out with a delay
            * If we're at the very last message to type out, leave message
            * Else, commence erase effect and repeat for next message
            */
            function typeEffect() {
                if (arrayPos.value == messages.value.length-1) {
                    if (stringPos.value == messages.value[arrayPos.value].length) {
                        typing.value = false
                        setTimeout(showImage, delayTimer.value-500)
                    }
                    else {
                        if (!typing.value) typing.value = true
                        welcomeMessage.value += messages.value[arrayPos.value].charAt(stringPos.value)
                        stringPos.value++
                        setTimeout(typeEffect, typingSpeed.value)
                    }
                }
                else if (stringPos.value < messages.value[arrayPos.value].length) {
                    if (!typing.value) typing.value = true
                    welcomeMessage.value += messages.value[arrayPos.value].charAt(stringPos.value)
                    stringPos.value++
                    setTimeout(typeEffect, typingSpeed.value)
                }
                else {
                    typing.value = false
                    if (arrayPos.value < messages.value.length) {
                        setTimeout(eraseEffect, delayTimer.value)
                    }
                }
            }
            /* 
            * method for controlling the erase effect 
            * welcomeMessage: array holding the strings to print
            * arrayPos: current position of the array
            * stringPos: current position of the string in welcomeMessage
            * typing: boolean to control whether or not we are still creating welcomeMessage
            * 
            * Slowly empty welcomeMessage until string is empty with a delay
            * When empty, commence typing effect and repeat for next message
            */
            function eraseEffect() {
                if (stringPos.value > 0) {
                    if (!typing.value) typing.value = true;
                    welcomeMessage.value = messages.value[arrayPos.value].substring(0, stringPos.value-1)
                    stringPos.value--;
                    setTimeout(eraseEffect, erasingSpeed.value);
                }
                else {
                    typing.value = false;
                    arrayPos.value++;
                    if (arrayPos.value < messages.value.length) {
                        setTimeout(typeEffect, delayTimer.value);
                    }
                }
            }
            /*
            * method just to delay and trigger image
            */ 
            function showImage() {
                welcomeFinished.value = true;
                setTimeout(showButton, delayTimer.value-200)
            }
            /*
            * method just to delay and show button
            */ 
            function showButton() {
                pictureVisible.value = true;
            }

            
            /* lifestyle hooks */
            // onMounted() runs once the component is mounted onto the page
            onMounted(() => setTimeout(typeEffect, delayTimer.value + 200))
            return { isMobile, welcomeMessage, typing, welcomeFinished,
                 pictureVisible, reveal, revealLeft, revealRight }
        }
    }

</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    /* needed to make the div fill entire screen on the left side  */
    div {
        margin-left: calc(-50vw + 50%);
    }
    /* css for the positioning of the welcome component */
    #mobile {
        position: fixed;
        top: 0;
        width: 100vw;
        height: 100vh;
        left: 0%;
        right: 0%;
        background-color: #0e181f;
        z-index: 1;

    }
    #mobile-welcome {
        position: fixed;
        top: 0;
        width: 100vw;
        height: 100vh;
        left: 0%;
        right: 0%;
        background-attachment: fixed;
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        background-image: url('../assets/welcome-waves.svg');
        z-index: 2;
    }
    #desktop-welcome {
        position: fixed;
        top: 0;
        width: 100vw;
        height: 100vh;
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        background-image: url('../assets/welcome-waves.svg');
    }
    /* css of pic of me */
    #mobile-pic {
        position: fixed;
        border: 2px solid #5368b0;
        top: 2vh;
        height: 40vw;
        width: 40vw;
        left: 30vw;
        border-radius:50% 50% 50% 50%; 
    }
    #desktop-pic {
        position: absolute;
        border: 2px solid #5368b0;
        top: 1vh;
        height: 20vw;
        width: 20vw;
        left: 40vw;
        border-radius:50% 50% 50% 50%; 
    }
    @keyframes bounce-in {
        0% {
            opacity: 0;
            transform: scale(.3);
        }
        50% {
            opacity: 1;
            transform: scale(1.05);
        }
        70% { 
            transform: scale(.9); 
        }
        100% { 
            transform: scale(1);
        }
    }
    @-moz-keyframes bounce-in {
        0% {
            opacity: 0;
            transform: scale(.3);
        }
        50% {
            opacity: 1;
            transform: scale(1.05);
        }
        70% { 
            transform: scale(.9); 
        }
        100% { 
            transform: scale(1);
        }
    }
    @-webkit-keyframes bounce-in {
        0% {
            opacity: 0;
            transform: scale(.3);
        }
        50% {
            opacity: 1;
            transform: scale(1.05);
        }
        70% { 
            transform: scale(.9); 
        }
        100% { 
            transform: scale(1);
        }
    }
    @-ms-keyframes bounce-in {
        0% {
            opacity: 0;
            transform: scale(.3);
        }
        50% {
            opacity: 1;
            transform: scale(1.05);
        }
        70% { 
            transform: scale(.9); 
        }
        100% { 
            transform: scale(1);
        }
    }
    @-o-keyframes bounce-in {
        0% {
            opacity: 0;
            transform: scale(.3);
        }
        50% {
            opacity: 1;
            transform: scale(1.05);
        }
        70% { 
            transform: scale(.9); 
        }
        100% { 
            transform: scale(1);
        }
    }
    /* css for the text being written on screen */
    .mobile-welcome-text {
        position: relative;
        color: #5368b0;
        top: 35%;
        font-size: 5vw;
        font-weight: normal;
        justify-content: center;
        align-items: center;
        text-align: center;
    }
    .desktop-welcome-text {
        position: relative;
        color: #5368b0;
        top: 40%;
        font-size: 3vw;
        font-weight: normal;
        justify-content: center;
        align-items: center;
        text-align: center;
    }
    /* css for the simulated typing cursor */
    .mobile-cursor {
        position: relative;
        top: 35%;
        font-size: 5vw;
        color: #5368b0;
        -webkit-animation: 1s blink step-end infinite;
        -moz-animation: 1s blink step-end infinite;
        -ms-animation: 1s blink step-end infinite;
        -o-animation: 1s blink step-end infinite;
        animation: 1s blink step-end infinite;  
    }
    .desktop-cursor {
        position: relative;
        top: 40%;
        font-size: 3vw;
        color: #5368b0;
        -webkit-animation: 1s blink step-end infinite;
        -moz-animation: 1s blink step-end infinite;
        -ms-animation: 1s blink step-end infinite;
        -o-animation: 1s blink step-end infinite;
        animation: 1s blink step-end infinite;   
    }
    @keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #5368b0;
        }
    }
    @-moz-keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #5368b0;
        }
    }
    @-webkit-keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #5368b0;
        }
    }
    @-ms-keyframes  blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #5368b0;
        }
    }
    @-o-keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #5368b0;
        }
    }
    /* css for the transition trigger */
    .mobile-wrapper {
        top: 40%;
        left: 25%;
        font-size: 5vw;
        width: 30vw;
        display: table;
        font-weight: normal;
        text-align: center;
        position: absolute;
        justify-content: center;
    }
    .desktop-wrapper {
        top: 50%;
        left: 35%;
        font-size: 3vw;
        width: 30vw;
        display: table;
        font-weight: normal;
        text-align: center;
        position: absolute;
        justify-content: center;
    }
    #mobile-transition-trigger {
        margin: 0;
        padding: 0;
        width: 33.33%;
        color: #5368b0;
        white-space: nowrap;
        -webkit-animation: expand-center 0.5s linear;
        -moz-animation: expand-center 0.5s linear;
        -ms-animation: expand-center 0.5s linear;
        -o-animation: expand-center 0.5s linear;
        animation: expand-center 0.5s linear;
    }
    #mobile-transition-trigger-left {
        margin: 0;
        padding: 0;
        position: fixed;
        color: #5368b0;
        width: 33.33%;
        -webkit-animation: mobile-slide-left 0.5s linear forwards;
        -moz-animation: mobile-slide-left 0.5s linear forwards;
        -ms-animation: mobile-slide-left 0.5s linear forwards;
        -o-animation: mobileslide-left 0.5s linear forwards;
        animation: mobile-slide-left 0.5s linear forwards;
    }
    #mobile-transition-trigger-right {
        margin: 0;
        padding: 0;
        position: fixed;
        color: #5368b0;
        width: 33.33%;
        -webkit-animation: mobile-slide-right 0.5s linear forwards;
        -moz-animation: mobile-slide-right 0.5s linear forwards;
        -ms-animation: mobile-slide-right 0.5s linear forwards;
        -o-animation: mobile-slide-right 0.5s linear forwards;
        animation: mobile-slide-right 0.5s linear forwards;
    }
    #desktop-transition-trigger {
        width: 33.33%;
        color: #5368b0;
        white-space: nowrap;
        -webkit-animation: expand-center 0.5s linear;
        -moz-animation: expand-center 0.5s linear;
        -ms-animation: expand-center 0.5s linear;
        -o-animation: expand-center 0.5s linear;
        animation: expand-center 0.5s linear;
    }
    #desktop-transition-trigger-left {
        position: fixed;
        color: #5368b0;
        width: 33.33%;
        -webkit-animation: desktop-slide-left 0.5s linear forwards;
        -moz-animation: desktop-slide-left 0.5s linear forwards;
        -ms-animation: desktop-slide-left 0.5s linear forwards;
        -o-animation: desktop-slide-left 0.5s linear forwards;
        animation: desktop-slide-left 0.5s linear forwards;
    }
    #desktop-transition-trigger-right {
        position: fixed;
        color: #5368b0;
        width: 33.33%;
        -webkit-animation: desktop-slide-right 0.5s linear forwards;
        -moz-animation: desktop-slide-right 0.5s linear forwards;
        -ms-animation: desktop-slide-right 0.5s linear forwards;
        -o-animation: desktop-slide-right 0.5s linear forwards;
        animation: desktop-slide-right 0.5s linear forwards;
    }
    /* mobile slides */
    @keyframes mobile-slide-left {
        from {
            transform: translateX(20%)
        }
        to {
            transform: translateX(-55%)
        }
    }
    @-moz-keyframes mobile-slide-left {
        from {
            transform: translateX(20)
        }
        to {
            transform: translateX(-55%)
        }
    }
    @-webkit-keyframes mobile-slide-left {
        from {
            transform: translateX(20%)
        }
        to {
            transform: translateX(-55%)
        }
    }
    @-ms-keyframes mobile-slide-left {
        from {
            transform: translateX(20%)
        }
        to {
            transform: translateX(55%)
        }
    }
    @-o-keyframes mobile-slide-left {
        from {
            transform: translateX(20%)
        }
        to {
            transform: translateX(55%)
        }
    }
    @keyframes mobile-slide-right {
        from {
            transform: translateX(-120%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-moz-keyframes mobile-slide-right {
        from {
            transform: translateX(-120%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-webkit-keyframes mobile-slide-right {
        from {
            transform: translateX(-120%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-ms-keyframes mobile-slide-right {
        from {
            transform: translateX(-120%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-o-keyframes mobile-slide-right {
        from {
            transform: translateX(-120%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    /* desktop slides */
    @keyframes desktop-slide-left {
        from {
            transform: translateX(-15%)
        }
        to {
            transform: translateX(-55%)
        }
    }
    @-moz-keyframes desktop-slide-left {
        from {
            transform: translateX(-15%)
        }
        to {
            transform: translateX(-55%)
        }
    }
    @-webkit-keyframes desktop-slide-left {
        from {
            transform: translateX(-15%)
        }
        to {
            transform: translateX(-55%)
        }
    }
    @-ms-keyframes desktop-slide-left {
        from {
            transform: translateX(-15%)
        }
        to {
            transform: translateX(55%)
        }
    }
    @-o-keyframes desktop-slide-left {
        from {
            transform: translateX(-10%)
        }
        to {
            transform: translateX(55%)
        }
    }
    @keyframes desktop-slide-right {
        from {
            transform: translateX(-90%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-moz-keyframes desktop-slide-right {
        from {
            transform: translateX(-90%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-webkit-keyframes desktop-slide-right {
        from {
            transform: translateX(-90%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-ms-keyframes desktop-slide-right {
        from {
            transform: translateX(-90%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @-o-keyframes desktop-slide-right {
        from {
            transform: translateX(-90%)
        }
        to {
            transform: translateX(-47%)
        }
    }
    @keyframes expand-center {
        0% {
            clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
            -webkit-clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
        }
        100% {
            clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
            -webkit-clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
        }
    }
    @-moz-keyframes expand-center {
        0% {
            clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
            -webkit-clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
        }
        100% {
            clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
            -webkit-clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
        }
    }
    @-webkit-keyframes expand-center {
        0% {
            clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
            -webkit-clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
        }
        100% {
            clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
            -webkit-clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
        }
    }
    @-ms-keyframes expand-center {
        0% {
            clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
            -webkit-clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
        }
        100% {
            clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
            -webkit-clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
        }
    }
    @-o-keyframes expand-center {
        0% {
            clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
            -webkit-clip-path: polygon(50% 100%,50% 0,50% 0,50% 100%);
        }
        100% {
            clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
            -webkit-clip-path: polygon(0 100%, 0 0, 100% 0, 100% 100%);
        }
    }
</style>