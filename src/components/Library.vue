<script>
import CardView from "./CardView.vue";
    class Card{
    constructor(q, a, id) {
        this.id = id
        this.question = q
        this.answer = a
    }
    }
    class CardDeck{
    constructor(name, creator, id) {
        this.id = id
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
    export default {
    name: "Library",
    props: ["data", "user"],
    data() {
        return {
            decks: [],
            animations: {
                createPush: false,
                createPull: false,
                importPush: false,
                importPull: false
            },
            activeDeck: null,
            newCardName: null,
            renderDeck: false
        };
    },
    mounted() {
        if (this.data){
            this.data.forEach(x => {
                this.decks.push(x);
            });
        }
    },
    methods: {
        handleButton(string) {
            if (!this.animations[`${string}Push`]) {
                this.animations[`${string}Push`] = true;
                this.animations[`${string}Pull`] = false;
            }
            else {
                this.animations[`${string}Pull`] = true;
                this.animations[`${string}Push`] = false;
            }
        },
        returner() {
            this.$emit("return");
        },
        setActiveDeck(deck) {
            this.activeDeck = deck;
            this.renderDeck = true;
        },
        createDeck() {
            this.decks.push(new CardDeck(this.newCardName, this.user, this.decks.length + 1));
            this.newCardName = null;
            this.handleButton("create");
            this.$emit("newdeck", this.decks);
        },
        importDeck() {
            let obj = JSON.parse(`{}`);
        },
        handleCardViewSave(deck) {
            this.decks.splice(deck.id - 1, 1, deck)
            localStorage.setItem('CardDecks', JSON.stringify({User: this.user, cardDecks: this.decks}))
        },
        handleReturnfromCardView() {
            this.renderDeck = false
            this.animations = {
                createPush: false,
                createPull: false,
                importPush: false,
                importPull: false
            }
        },
        handleDeleteDeck(deck) {
            this.decks = this.decks.filter(x => x.id != deck.id)
            this.animations = {
                createPush: false,
                createPull: false,
                importPush: false,
                importPull: false
            }
            this.renderDeck = false
            this.decks.forEach((x, i) => {
                x.id = i + 1
            })
            localStorage.setItem('CardDecks', JSON.stringify({User: this.user, cardDecks: this.decks}))
        }
    },
    emits: ["return", "newdeck"],
    components: { CardView }
}
</script>

<template>
    <div class="backgrounddiv" v-if="!renderDeck">
        <h1 style="position: fixed; top: 0vh; font-size: 4rem">Your Library</h1>
        <div class="button" style="left: 5vh; top: 5vh;" @click="handleButton('create')">Create</div>
        <div class="deckdiv">
            <div v-for="deck in decks">
                <div class="singledeck" @click="setActiveDeck(deck)">
                    {{ deck.name }}
                </div>
            </div>
        </div>
        <div v-if="animations['createPush']" style="width: 150vw; height: 150vh; position: fixed; z-index: 2;" @click="handleButton('create')"></div>
        <div v-if="animations['importPush']" style="width: 150vw; height: 150vh; position: fixed; z-index: 2;" @click="handleButton('import')"></div>
        <div class="creatediv" v-bind:class="{animateUp: animations.createPush, animateDown: animations.createPull}">
            <nav class="navbar">Create New Deck</nav>
            <h4 style="color: purple; font-size: 5vh; margin-top: -1vh;">Name:<input type="text" placeholder="Enter Deck Name" style="height: 5vh; margin-left: 2vw; border-radius: 1vh;" v-model="newCardName"></h4>
            <div class="button" style="background-color: rgb(189, 0, 189); color: orange; position: absolute; bottom: 10vh;" @click="createDeck()">Submit</div>
        </div>
        <div class="creatediv" v-bind:class="{animateUp: animations.importPush, animateDown: animations.importPull}">
            <nav class="navbar">Import Deck</nav>
            <h4 style="color: purple; font-size: 5vh; margin-top: -1vh;">Code:<input type="text" placeholder="Enter Code" style="height: 5vh; margin-left: 2vw; border-radius: 1vh;" v-model="newCardName"></h4>
            <div class="button" style="background-color: rgb(189, 0, 189); color: orange; position: absolute; bottom: 10vh;" @click="importDeck()">Submit</div>
        </div>
        <div class="button" style="right: 5vh; top: 5vh;" @click="handleButton('import')">Import</div>
        <div class="button" style="right: 5vh; bottom: 3vh;" @click="returner()">return</div>
    </div>
    <CardView v-if="renderDeck" :data="activeDeck" @save="x => handleCardViewSave(x)" @return="handleReturnfromCardView()" @delete="x => handleDeleteDeck(x)"/>
</template>

<style scoped>
    .backgrounddiv{
        background-color: rgb(189, 0, 189);
        position: absolute;
        left: 0px;
        top: 0px;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
        width: 100vw;
        overflow: hidden;
        color: orange;
    }
    .button{
        cursor: pointer;
        font-size: 1rem;
        width: 7vw;
        height: 7vh;
        border-radius: 2vh;
        display: flex;
        justify-content: center;
        align-items: center;
        box-sizing: border-box;
        background-clip: content-box;
        position: absolute;
        background-color: orange;
        color: purple;
    }
    .creatediv{
        position: fixed;
        height: 65vh;
        width: 45vw;
        background-color: orange;
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        border-radius: 2vh;
        border: 1vh solid rgb(127, 2, 185);
        top: 150vh;
        z-index: 3;
        color: purple;
    }
    @keyframes animateUp{
        0% {top: 150vh;}
        100% {transform: translateY(-133vh);}
    }
    .animateUp{
        animation: animateUp 1.5s forwards;
    }
    @keyframes animateDown{
        0% {transform: translateY(-133vh);}
        100% {top: 150vh;}
    }
    .animateDown{
        animation: animateDown 1.5s forwards;
    }
    .navbar{
        height: 20%;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 2.5rem;
        border-bottom: 0.5vh solid rgb(127, 2, 185);
        cursor: default;
    }
    .deckdiv{
        background-color: rgb(127, 2, 185);
        height: 66vh;
        width: 90vw;
        margin-top: 6vh;
        border-radius: 3vh;
        border: 1vh solid purple;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 0vh 13vh;
        overflow-y: scroll;
    }
    .singledeck{
        margin-top: 3vh;
        width: 25vw;
        height: 25vw;
        background-color: black;
        margin-left: 1vh;
        margin-bottom: 3vh;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: orange;
        color: purple;
        border-radius: 2vh;
        font-size: 3rem;
    }
    ::-webkit-scrollbar{
        width: 0px;
        height: 0px;
    }
</style>