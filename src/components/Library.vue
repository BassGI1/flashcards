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
                importPull: false,
                generatePush: false,
                generatePull: false
            },
            activeDeck: null,
            newCardName: null,
            renderDeck: false,
            coded: false,
            code: null,
            importText: null
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
            setTimeout(() => this.coded = false, 1000)
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
            let temp = this.importText
            temp = temp.substring(16).slice(0, -1)
            temp = JSON.parse(`{"id":${this.decks.length + 1},"name":"${this.dehash(temp)}`)
            this.decks.push(temp)
            this.importText = null
            this.handleButton("import")
            this.$emit("newdeck", this.decks)
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
                importPull: false,
                generatePush: false,
                generatePull: false
            }
        },
        handleDeleteDeck(deck) {
            this.decks = this.decks.filter(x => x.id != deck.id)
            this.animations = {
                createPush: false,
                createPull: false,
                importPush: false,
                importPull: false,
                generatePush: false,
                generatePull: false
            }
            this.renderDeck = false
            this.decks.forEach((x, i) => {
                x.id = i + 1
            })
            localStorage.setItem('CardDecks', JSON.stringify({User: this.user, cardDecks: this.decks}))
        },
        generateRandomString(numChars) {
            const regex = `qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM!@#$%^&*()_+-[]{}"',.: 1234567890`
            const length = regex.length
            let str = ''
            for (let x = 0; x < numChars; ++x){
                str += regex[Math.floor(Math.random()*length)]
            }
            return str
        },
        hash(string) {
            const regex = `qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM!@#$%^&*()_+-[]{}"',.: 1234567890`
            const length = regex.length
            let newString = ''
            for (let i = 0; i < string.length; ++i){
                newString += regex[(regex.indexOf(string[i]) + 20)%length]
            }
            return newString
        },
        dehash(string) {
            const regex = `qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM!@#$%^&*()_+-[]{}"',.: 1234567890`
            const length = regex.length
            let newString = ''
            for (let i = 0; i < string.length; ++i){
                let index = regex.indexOf(string[i])
                if (index < 20){
                    let diff = 20 - index
                    index = length - diff
                }
                else{
                    index = index - 20
                }
                newString += regex[index]
            }
            return newString
        },
        generateCode(deck) {
            this.coded = true
            let temp = JSON.stringify({...deck, id: 0})
            this.code = `${this.generateRandomString(16)}${this.hash(temp.substring(16))}`
            this.code.slice(0, -1)
            this.code += this.generateRandomString(1)
        },
        copyText() {
            navigator.clipboard.writeText(this.code)
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
                    <h4 style="font-weight: lighter; display: inline-block; word-break: break-all; margin: 2vw; text-align: center;">{{ deck.name }}</h4>
                </div>
            </div>
        </div>
        <div v-if="animations['createPush']" style="width: 150vw; height: 150vh; position: fixed; z-index: 2;" @click="handleButton('create')"></div>
        <div v-if="animations['importPush']" style="width: 150vw; height: 150vh; position: fixed; z-index: 2;" @click="handleButton('import')"></div>
        <div v-if="animations['generatePush']" style="width: 150vw; height: 150vh; position: fixed; z-index: 2;" @click="handleButton('generate')"></div>
        <div class="creatediv" v-bind:class="{animateUp: animations.createPush, animateDown: animations.createPull}">
            <nav class="navbar">Create New Deck</nav>
            <h4 style="color: purple; font-size: 5vh; margin-top: -1vh;">Name:<input type="text" placeholder="Enter Deck Name" style="height: 5vh; margin-left: 2vw; border-radius: 1vh;" v-model="newCardName"></h4>
            <div class="button" style="background-color: rgb(189, 0, 189); color: orange; position: absolute; bottom: 10vh;" @click="createDeck()">Submit</div>
        </div>
        <div class="creatediv" v-bind:class="{animateUp: animations.importPush, animateDown: animations.importPull}">
            <nav class="navbar">Import Deck</nav>
            <h4 style="color: purple; font-size: 5vh; margin-top: -1vh;">Code:<input type="text" placeholder="Enter Code" style="height: 5vh; margin-left: 2vw; border-radius: 1vh;" v-model="importText"></h4>
            <div class="button" style="background-color: rgb(189, 0, 189); color: orange; position: absolute; bottom: 10vh;" @click="importDeck()">Submit</div>
        </div>
        <div class="button" style="right: 5vh; top: 5vh;" @click="handleButton('import')">Import</div>
        <div class="button" style="right: 5vh; bottom: 3vh;" @click="returner()">Return</div>
        <div class="button" style="left: 5vh; bottom: 3vh; text-align: center;" @click="handleButton('generate')">Share</div>
        <div class="creatediv" v-bind:class="{animateUp: animations.generatePush, animateDown: animations.generatePull}">
            <nav class="navbar" v-if="!coded">Which deck would you like to share?</nav>
            <div style="width: 100%; display: flex; align-items: center; justify-content: center; flex-wrap: wrap; height: 60%; overflow-y: scroll;" v-if="!coded">
                <div v-for="deck in decks" style="width: 100%; display: flex; align-items: center; justify-content: center; flex-wrap: wrap; margin: 1vh; background-color: purple; cursor: pointer; color: orange; border-radius: 1vh;" @click="generateCode(deck)">
                    {{deck.name}}
                </div>
            </div>
            <nav class="navbar" v-if="coded">Your Code is:</nav>
            <div style="display: inline-block; word-break: break-word; margin: 2vw;">
                <p v-if="coded" style="cursor: pointer;" @click="copyText()">{{code}}</p>
                <h2 v-if="coded" style="text-align: center; margin-top: 5vh;">Click to copy to clipboard</h2>
            </div>
        </div>
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