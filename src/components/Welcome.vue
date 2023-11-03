<template>
    <div id="mobile" v-if="isMobile()">
        <div id="mobile-welcome">
            <img v-if="typingDone" id="mobile-pic" :src="image" />
            <span class="mobile-welcome-text">{{ welcomeMessage }}<span class="mobile-cursor">|</span></span>
        </div>
    </div>
    <div id="not-mobile" v-else>
        <div id="desktop-welcome">
                <img v-if="typingDone" id="desktop-pic" :src="image" />
                <span class="desktop-welcome-text">{{ welcomeMessage }}<span class="desktop-cursor">|</span></span>
        </div>
    </div>
</template>
  
<script>
    import { ref, onMounted } from 'vue'
    import image from '../assets/self.png'

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
            let typingDone = ref(false)
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
                typingDone.value = true;
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
            return { isMobile, welcomeMessage, typing, image, typingDone, 
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
        top: 4vh;
        height: 50vw;
        width: 50vw;
        left: 25vw;
        border-radius: 50% 50% 50% 50%; 
        -webkit-animation: 1s bounce ease;
        -moz-animation: 1s bounce ease;
        -ms-animation: 1s bounce ease;
        -o-animation: 1s bounce ease;
        animation: 1s bounce ease; 
    }
    #desktop-pic {
        position: fixed;
        border: 2px solid #5368b0;
        top: 1vh;
        height: 20vw;
        width: 20vw;
        left: 40vw;
        border-radius: 50% 50% 50% 50%; 
        -webkit-animation: 1s bounce ease;
        -moz-animation: 1s bounce ease;
        -ms-animation: 1s bounce ease;
        -o-animation: 1s bounce ease;
        animation: 1s bounce ease; 
    }
    @keyframes bounce {
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
    @-moz-keyframes bounce {
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
    @-webkit-keyframes bounce {
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
    @-ms-keyframes bounce {
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
    @-o-keyframes bounce {
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
        display: inline-block;
        color: #5368b0;
        font-size: 6vw;
        font-weight: normal;
        transform: translateY(45vh);
    }
    .desktop-welcome-text {
        display: inline-block;
        vertical-align: middle;
        color: #5368b0;
        font-size: 3vw;
        font-weight: normal;
        transform: translateY(45vh);
    }
    /* css for the simulated typing cursor */
    .mobile-cursor {
        font-size: 6vw;
        color: #5368b0;
        -webkit-animation: 1s blink step-end infinite;
        -moz-animation: 1s blink step-end infinite;
        -ms-animation: 1s blink step-end infinite;
        -o-animation: 1s blink step-end infinite;
        animation: 1s blink step-end infinite;  
    }
    .desktop-cursor {
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
</style>