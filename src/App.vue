<script>
  export default {
    data() {
      return {
        data: JSON.parse(localStorage.getItem('CardDecks')),
        renderWelcome: true,
        User: null,
        animateButton: false
      }
    },
    mounted() {
      this.CheckIfNewUser()
    },
    methods: {
      CheckIfNewUser() {
        if (this.data) {
          this.User = this.data.User
          this.renderWelcome = false
        }
      },
      FinishWelcome() {
        this.animateButton = true
        setTimeout(() => {
          this.animateButton = false
          this.renderWelcome = false
        }, 5000)
      }
    }
  }
</script>

<template>
  <div class="app">
    
    <div v-if="renderWelcome" class="welcome">
      <h1 v-if="renderWelcome" style="width: 100%; text-align: center; margin-top: -10vh;">Hello! You're new here! What's your name?</h1>
      <input v-if="!animateButton" type="text" placeholder="Enter Your Name" v-model="User" class="inputName">
      <input v-if="animateButton" type="text" placeholder="Enter Your Name" class="inputName" :value="User" onkeydown="return false">
      <div class="startbutton" @click="FinishWelcome()" v-bind:class="{animateButton: animateButton}"> {{ !animateButton ? 'Submit' : '' }} </div>
    </div>

    <div v-if="!renderWelcome">
      {{User}}
    </div>

  </div>
</template>

<style scoped>
  .app{
    color: rgb(189, 0, 189);
    font-family: 'Pangolin', cursive;
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    left: 0px;
    top: 0px;
    min-height: 100vh;
    min-width: 100vw;
    overflow: hidden;
    background-color: orange;
  }
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
    background-color: blueviolet;
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
  }
</style>