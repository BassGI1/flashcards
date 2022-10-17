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
    }
import Welcome from './components/Welcome.vue'
import DisplayCard from './components/DisplayCard.vue';
import TutorialSection from './components/TutorialSection.vue'
import Library from './components/Library.vue'
  export default {
    data() {
        return {
            data: JSON.parse(localStorage.getItem("CardDecks")),
            renderWelcome: false,
            User: null,
            animateButton: false,
            renderHello: true,
            currentTab: ''
        };
    },
    mounted() {
        this.CheckIfNewUser();
        setTimeout(() => {
          this.renderHello = false
        }, 3000);
        // this.data.cardDecks.push(new CardDeck('test', this.User))
        // localStorage.setItem("CardDecks", JSON.stringify(this.data))
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
        },
        tabChange(tab) {
          this.currentTab = tab
        },
        newDeck(x) {
          this.data.cardDecks = x
          localStorage.setItem('CardDecks', JSON.stringify(this.data))
        },
        handleReturnfromWelcome(x) {
          this.data = {User: x, cardDecks: []}
          this.renderWelcome = false
          this.User = x
          localStorage.setItem('CardDecks', JSON.stringify(this.data))
        }
    },
    components: { Welcome, DisplayCard, TutorialSection, Library }
}
</script>

<template>
  <div class="app">
    
    <Welcome v-if="renderWelcome" @return="x => handleReturnfromWelcome(x)"/>

    <div v-if="!renderWelcome" style="width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center; flex-wrap: wrap;">
      <h1 class="fade" v-if="renderHello">Welcome {{User}}!</h1>
      <div v-if="!renderHello && currentTab == ''" style="width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center; flex-wrap: wrap; position: absolute; left: 0px; top: 0px;">
        <DisplayCard :text="'Your Library'" @tabchange="tabChange('Your Library')"/>
        <DisplayCard :text="'Tutorial'" @tabchange="tabChange('Tutorial')"/>
        <DisplayCard :text="'Settings'" @tabchange="tabChange('Settings')"/>
        <DisplayCard :text="'About'" @tabchange="tabChange('About')"/>
      </div>
      <div style="width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; flex-wrap: wrap; background-color: rgb(189, 0, 189); color: orange;" v-if="!renderHello && currentTab == 'Tutorial'">
        <TutorialSection :Image="'tutorial1.png'" :Text="'Create Flash Card Decks!'"/>
        <TutorialSection :Image="'tutorial2.png'" :Text="'Create and Edit Cards in the Decks!'"/>
        <TutorialSection :Image="'tutorial3.png'" :Text="'Test Yourself with Test Mode!'"/>
        <TutorialSection :Image="'tutorial4.png'" :Text="'Share Your Creations with Friends!'"/>
        <div class="returnbutton" @click="tabChange('')">Exit Tutorial</div>
      </div>
      <Library v-if="!renderHello && currentTab == 'Your Library'" :data="data ? data.cardDecks : null" :user="User" @return="currentTab = ''" @newdeck="x => newDeck(x)"/>
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
  .returnbutton{
    position: absolute;
    bottom: 2vh;
    right: 2vh;
    cursor: pointer;
    font-size: 1rem;
    background-color: orange;
    color: purple;
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