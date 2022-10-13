<script>
  export default {
    data() {
      return {
        data: JSON.parse(localStorage.getItem('CardDecks')),
        User: null
      }
    },
    mounted() {
      
    },
    methods: {
      CheckIfNewUser() {
        if (data) {
          this.User = data.User
        }
      }
    }
  }
</script>

<template >
  <div class="app">
    <h1 v-if="!User">Hello! You're new here! What's your name?</h1>
  </div>
</template>

<style scoped>
  .app{
    position: absolute;
    left: 0px;
    top: 0px;
    min-height: 100vh;
    min-width: 100vw;
    overflow: hidden;
    background-color: orange;
  }
</style>