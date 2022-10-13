<script>
import Welcome from './components/Welcome.vue'
  export default {
    data() {
        return {
            data: JSON.parse(localStorage.getItem("CardDecks")) || {User: "Bassam"},
            renderWelcome: false,
            User: null,
            animateButton: false,
            renderHello: true
        };
    },
    mounted() {
        this.CheckIfNewUser();
        setTimeout(() => {
          this.renderHello = false
        }, 3000);
    },
    methods: {
        CheckIfNewUser() {
            if (this.data) {
                this.User = this.data.User;
                this.renderWelcome = false;
            }
            else{
              this.renderWelcome = true
            }
        }
    },
    components: { Welcome }
}
</script>

<template>
  <div class="app">
    
    <Welcome v-if="renderWelcome" @Return="(x) => {
      renderWelcome = false
      User = x
    }"/>

    <div v-if="!renderWelcome">
      <h1 class="fade" v-if="renderHello">Welcome {{User}}!</h1>
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
  .fade{
    font-size: 5rem;
    animation: fade 3s forwards;
  }
  @keyframes fade{
    0% {opacity: 1;}
    66.66% {opacity: 1;}
    100% {opacity: 0;}
  }
</style>