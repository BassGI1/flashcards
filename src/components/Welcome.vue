<script>
import TutorialSection from './TutorialSection.vue'
    export default {
    name: "Welcome",
    data() {
        return {
            animateButton: false,
            fade: false,
            stage2: false,
            tutorial: false,
            User: null
        };
    },
    methods: {
        FinishWelcome() {
            this.animateButton = true;
            setTimeout(() => {
                setTimeout(() => {
                    this.animateButton = false;
                    this.stage2 = true;
                    this.fade = false;
                }, 1000);
                this.fade = true;
            }, 5000);
        },
        returnToHomePage() {
            this.$emit("return", this.User);
        },
        handleNoTutorial() {
            this.fade = true;
            setTimeout(() => {
                this.returnToHomePage();
            }, 1000);
        },
        handleTutorial() {
            this.fade = true;
            setTimeout(() => {
                this.tutorial = true;
                this.fade = false;
            }, 1000);
        }
    },
    components: { TutorialSection },
    emits: ["return"]
}
</script>

<template>
    <div class="welcome" v-bind:class="{fade: fade}" v-if="!stage2">
      <h1 style="width: 100%; text-align: center; margin-top: -10vh;">Hello! You're new here! What's your name?</h1>
      <input v-if="!animateButton" type="text" placeholder="Enter Your Name" v-model="User" class="inputName">
      <input v-if="animateButton" type="text" placeholder="Enter Your Name" class="inputName" :value="User" onkeydown="return false">
      <div class="startbutton" @click="FinishWelcome()" v-bind:class="{animateButton: animateButton}"> {{ !animateButton ? 'Submit' : '' }} </div>
    </div>
    <div v-if="stage2 && !tutorial" class="welcome" v-bind:class="{fade: fade}">
        <h1 style="width: 100%; text-align: center; margin-top: -10vh;">Would you like to view the tutorial?</h1>
        <div class="button" @click="handleTutorial()">
            Yes
        </div>
        <div class="button" @click="handleNoTutorial()">
            No
        </div>
    </div>
    <div v-if="tutorial" style="display: flex; align-items: center; justify-content: center; width: 100vw; height: 100vh; overflow: hidden; flex-wrap: wrap;" v-bind:class="{fade: fade}">
        <TutorialSection :Image="'placeholder1.gif'"/>
        <TutorialSection :Image="'placeholder2.gif'"/>
        <TutorialSection :Image="'placeholder3.gif'"/>
        <TutorialSection :Image="'placeholder4.gif'"/>
        <div class="returnbutton" @click="handleNoTutorial()">Exit Tutorial</div>
    </div>
</template>

<style scoped>
  .welcome{
    font-size: x-large;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
  }
  .startbutton{
    position: absolute;
    bottom: 15vh;
    cursor: pointer;
    font-size: 3rem;
    background-color: rgb(175, 122, 225);
    width: 12vw;
    height: 12vh;
    border-radius: 2vh;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    background-clip: content-box
  }
  .animateButton{
    animation: animateButton 0.5s both, animateButtonColour 3s infinite;
  }
  @keyframes animateButton{
    0%{
      width: 12vw;
      height: 12vh;
    }
    100%{
      width: 4vw;
      height: 4vw;
      border-radius: 50%;
      padding: 2vw;
      box-shadow: inset 0 0 0 2vw purple;
    }
  }
  @keyframes animateButtonColour {
    0%{
      padding: 2vw;
      box-shadow: inset 0 0 0 1vw purple;
      cursor: default;
    }
    50%{
      padding: 2vw;
      box-shadow: inset 0 0 0 1vw rgb(15, 15, 147);
      cursor: default;
    }
    100%{
      padding: 2vw;
      box-shadow: inset 0 0 0 1vw purple;
      cursor: default;
    }
  }
  .inputName{
    margin-top: 10vh;
    width: 20vw;
    height: 5vh;
    font-size: large;
    font-family: 'Pangolin', cursive;
    border-radius: 1vh;
  }
  @keyframes fade{
    0% {opacity: 1;}
    100% {opacity: 0;}
  }
  .fade{
    animation: fade 1s forwards;
  }
  .button{
    margin: 2vw;
    cursor: pointer;
    font-size: 3rem;
    background-color: rgb(175, 122, 225);
    width: 12vw;
    height: 12vh;
    border-radius: 2vh;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    background-clip: content-box
  }
  .returnbutton{
    position: absolute;
    bottom: 2vh;
    right: 2vh;
    cursor: pointer;
    font-size: 1rem;
    background-color: rgb(175, 122, 225);
    width: 7vw;
    height: 7vh;
    border-radius: 2vh;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    background-clip: content-box
  }
</style>