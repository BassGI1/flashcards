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
    export default {
        name: "Library",
        props: ["data", "user"],
        data() {
            return {
                decks: [],
                createPush: false,
                createPull: false
            }
        },
        mounted() {
            this.data.forEach(x => {
                this.decks.push(x)
            })
        },
        methods: {
            handleCreate() {
                if (!this.createPush) {
                    this.createPush = true
                    this.createPull = false
                }
                else {
                    this.createPull = true
                    this.createPush = false
                }
            }
        }
    }
</script>

<template>
    <div class="backgrounddiv">
        <div class="button" style="left: 2vh; top: 2vh;" @click="handleCreate()">Create</div>
        <div v-for="deck in decks">
            {{deck.name}}
        </div>
        <div v-if="createPush" style="width: 150vw; height: 150vh; position: fixed; z-index: 2;" @click="handleCreate()"></div>
        <div class="creatediv" v-bind:class="{animateUp: createPush, animateDown: createPull}">
            <nav class="navbar">Create New Deck</nav>
        </div>
    </div>
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
</style>