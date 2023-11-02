<template>
    <div id="mobile" v-if="isMobile()">
        <div id="mobile-welcome">
            <span class="mobile-welcome-text">{{ welcomeMessage }}</span>
            <span class="mobile-cursor">|</span>
            <span id="mobile-transition-trigger-left" v-if="welcomeFinished">{{ revealLeft }}</span>
            <span id="mobile-transition-trigger" v-if="welcomeFinished">{{ reveal }}</span>
            <span id="mobile-transition-trigger-right" v-if="welcomeFinished">{{ revealRight }}</span>
        </div>
    </div>
    <div id="not-mobile" v-else>
        <div id="desktop-welcome">
            <span class="desktop-welcome-text">{{ welcomeMessage }}</span>
            <span class="desktop-cursor">|</span>
            <span id="desktop-transition-trigger-left" v-if="welcomeFinished">{{ revealLeft }}</span>
            <span id="desktop-transition-trigger" v-if="welcomeFinished">{{ reveal }}</span>
            <span id="desktop-transition-trigger-right" v-if="welcomeFinished">{{ revealRight }}</span>
        </div>
    </div>
</template>
  
<script>
    import { ref, onMounted } from 'vue'

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
                        welcomeFinished.value = true
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
            
            /* lifestyle hooks */
            // onMounted() runs once the component is mounted onto the page
            onMounted(() => setTimeout(typeEffect, delayTimer.value + 200))
            return { isMobile, welcomeMessage, typing, welcomeFinished, reveal, revealLeft, revealRight }
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
    #mobile-welcome {
        position: fixed;
        top: 0;
        background-color: #131516;
        height: 100%;
        width: 100%;
        z-index: 99;
    }
    #desktop-welcome {
        position: fixed;
        top: 0;
        background-color: #131516;
        height: 100%;
        width: 100%;
        z-index: 99;
    }
    /* css for the text being written on screen */
    .mobile-welcome-text {
        position: relative;
        top: 30%;
        font-size: 5vw;
        font-weight: normal;
        justify-content: center;
        align-items: center;
        text-align: center;
    }
    .desktop-welcome-text {
        position: relative;
        top: 30%;
        font-size: 3vw;
        font-weight: normal;
        justify-content: center;
        align-items: center;
        text-align: center;
    }
    /* css for the simulated typing cursor */
    .mobile-cursor {
        position: relative;
        top: 30%;
        font-size: 5vw;
        color: #2c3e50;
        -webkit-animation: 1s blink step-end infinite;
        -moz-animation: 1s blink step-end infinite;
        -ms-animation: 1s blink step-end infinite;
        -o-animation: 1s blink step-end infinite;
        animation: 1s blink step-end infinite;  
    }
    .desktop-cursor {
        position: relative;
        top: 30%;
        font-size: 3vw;
        color: #2c3e50;
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
            color: #2c3e50;
        }
    }
    @-moz-keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #2c3e50;
        }
    }
    @-webkit-keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #2c3e50;
        }
    }
    @-ms-keyframes  blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #2c3e50;
        }
    }
    @-o-keyframes blink {
        from,
        to {
            color: transparent;
        }
        50% {
            color: #2c3e50;
        }
    }
    /* css for the transition trigger */
    #mobile-transition-trigger {
        position: fixed;
        top: 35%;
        left: 25.5%;
        font-size: 5vw;
        font-weight: normal;
        text-align: center;
        -webkit-animation: expand-center 1s linear;
        -moz-animation: expand-center 1s linear;
        -ms-animation: expand-center 1s linear;
        -o-animation: expand-center 1s linear;
        animation: expand-center 1s linear;
    }
    #mobile-transition-trigger-left {
        position: fixed;
        top: 35%;
        left: 49%;
        font-size: 5vw;
        font-weight: normal;
        text-align: center;
        -webkit-animation: slide-left 1s linear forwards;
        -moz-animation: slide-left 1s linear forwards;
        -ms-animation: slide-left 1s linear forwards;
        -o-animation: slide-left 1s linear forwards;
        animation: slide-left 1s linear forwards;
    }
    #mobile-transition-trigger-right {
        position: fixed;
        top: 35%;
        left: 50%;
        font-size: 5vw;
        font-weight: normal;
        text-align: center;
        -webkit-animation: slide-right 1s linear forwards;
        -moz-animation: slide-right 1s linear forwards;
        -ms-animation: slide-right 1s linear forwards;
        -o-animation: slide-right 1s linear forwards;
        animation: slide-right 1s linear forwards;
    }
    #desktop-transition-trigger {
        position: fixed;
        top: 40%;
        left: 35%;
        font-size: 3vw;
        font-weight: normal;
        text-align: center;
        -webkit-animation: expand-center 1s linear;
        -moz-animation: expand-center 1s linear;
        -ms-animation: expand-center 1s linear;
        -o-animation: expand-center 1s linear;
        animation: expand-center 1s linear;
    }
    #desktop-transition-trigger-left {
        position: fixed;
        top: 40%;
        left: 49%;
        font-size: 3vw;
        font-weight: normal;
        text-align: center;
        -webkit-animation: slide-left 1s linear forwards;
        -moz-animation: slide-left 1s linear forwards;
        -ms-animation: slide-left 1s linear forwards;
        -o-animation: slide-left 1s linear forwards;
        animation: slide-left 1s linear forwards;
    }
    #desktop-transition-trigger-right {
        position: fixed;
        top: 40%;
        left: 50%;
        font-size: 3vw;
        font-weight: normal;
        text-align: center;
        -webkit-animation: slide-right 1s linear forwards;
        -moz-animation: slide-right 1s linear forwards;
        -ms-animation: slide-right 1s linear forwards;
        -o-animation: slide-right 1s linear forwards;
        animation: slide-right 1s linear forwards;
    }
    @keyframes slide-left {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(-2000%)
        }
    }
    @-moz-keyframes slide-left {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(-2000%)
        }
    }
    @-webkit-keyframes slide-left {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(-2000%)
        }
    }
    @-ms-keyframes slide-left {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(-2000%)
        }
    }
    @-o-keyframes slide-left {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(-2000%)
        }
    }
    @keyframes slide-right {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(1950%)
        }
    }
    @-moz-keyframes slide-right {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(1950%)
        }
    }
    @-webkit-keyframes slide-right {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(1950%)
        }
    }
    @-ms-keyframes slide-right {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(1950%)
        }
    }
    @-o-keyframes slide-right {
        from {
            transform: translateX(0%)
        }
        to {
            transform: translateX(1950%)
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