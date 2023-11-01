<template>
    <div id="mobile" v-if="isMobile()">
        <div id="mobile-welcome">
            <div id="mobile-welcome-text-properties">
                <span class="mobile-welcome-text">{{ welcomeMessage }}</span>
                <span class="mobile-cursor">|</span>
                <span class="cursor" :class="{ typing: typing }">&nbsp;</span>
            </div>
        </div>
    </div>
    <div id="not-mobile" v-else>
        <div id="desktop-welcome">
            <div id="desktop-welcome-text-properties">
                <span class="desktop-welcome-text">{{ welcomeMessage }}</span>
                <span class="desktop-cursor">|</span>
                <span class="cursor" :class="{ typing: typing }">&nbsp;</span>
            </div>
        </div>
    </div>
</template>
  
<script>
    import { ref, onMounted } from 'vue'

    export default {
        name: "Welcome",
        // setup() runs prior to any html being loaded onto the page 
        setup() {
            const isMobile = () => navigator.userAgent.includes("Mobi") ? true : false

            const messages = ref(["Welcome to my portfolio!", "Scroll to begin exploring."])
            const typingSpeed = ref(100)
            const delayTimer = ref(1000)
            let welcomeMessage = ref("")
            let typing = ref(false)
            let welcomeFinished = ref(false)
            let arrayPos = ref(0)
            let stringPos = ref(0)
            
            /* method for controlling the typing effect
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
            /* method for controlling the erase effect 
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
                    setTimeout(eraseEffect, typingSpeed.value);
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
            return { isMobile, welcomeMessage, typing, welcomeFinished }
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
    #mobile-welcome-text-properties span.mobile-welcome-text {
        font-size: 5vw;
        font-weight: normal;
        justify-content: center;
        align-items: center;
    }
    #mobile-welcome-text {
        margin-left: auto;
        margin-right: auto;
        text-align: center;
    }
    .mobile-cursor {
        font-size: 5vw;
        color: #2c3e50;
        -webkit-animation: 1s blink step-end infinite;
        -moz-animation: 1s blink step-end infinite;
        -ms-animation: 1s blink step-end infinite;
        -o-animation: 1s blink step-end infinite;
        animation: 1s blink step-end infinite;   
    }
    #desktop-welcome {
        position: fixed;
        top: 0;
        background-color: #131516;
        height: 100%;
        width: 100%;
        z-index: 99;
    }
    #desktop-welcome-text-properties span.desktop-welcome-text {
        font-size: 3vw;
        font-weight: normal;
        justify-content: center;
        align-items: center;
    }
    .desktop-welcome-text {
        margin-left: auto;
        margin-right: auto;
        text-align: center;
    }
    .desktop-cursor {
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
    @-ms-keyframes blink {
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
</style>