<script>
class Card{
  constructor(q, a, id) {
    this.id = id
    this.question = q
    this.answer = a
  }
}
class CardDeck{
  constructor(name, creator) {
    this.name = name
    this.creator = creator
    this.cards = []
  }
  newCard(question, answer) {
    this.cards.push(new Card(question, answer, this.cards.length + 1))
  }
  updateIds() {
    this.cards = this.cards.filter(card => card != undefined)
    this.cards.forEach((x, i) => {
      x.id = i + 1
    })
  }
  deleteCard(id) {
    this.cards = this.cards.map(card => {
      if (card.id != id){
        return card
      }
    })
    this.updateIds()
  }
  moveCard(id, position) {
      let card = this.cards.filter(x => x.id == id)[0]
      this.deleteCard(id)
      this.cards.splice(position - 1, 0, card)
      this.updateIds()
  }
}
import Welcome from './components/Welcome.vue'
import DisplayCard from './components/DisplayCard.vue';
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
    components: { Welcome, DisplayCard }
}
</script>

<template>
  <div class="app">
    
    <Welcome v-if="renderWelcome" @Return="(x) => {
      renderWelcome = false
      User = x
    }"/>

    <div v-if="!renderWelcome" style="width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center; flex-wrap: wrap;">
      <h1 class="fade" v-if="renderHello">Welcome {{User}}!</h1>
      <div v-if="!renderHello" style="width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center; flex-wrap: wrap;">
        <DisplayCard />
        <DisplayCard />
      </div>
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