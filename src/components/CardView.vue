<script>
class Card{
    constructor(q, a, id) {
        this.id = id
        this.question = q
        this.answer = a
    }
    }
    export default {
        name: "CardView",
        props: ["data"],
        data() {
            return {
                deck: null,
                currentCard: this.data.cards[0] || null,
                flipped: false
            }
        },
        mounted() {
            this.deck = {...this.data}
            console.log(this.deck.cards)
        },
        methods: {
            newCard(question, answer) {
                this.deck.cards.push(new Card(question, answer, this.deck.cards.length + 1))
            }
        }
    }
</script>

<template>
    <div class="backgrounddiv">
        <nav class="topbar">
            <div class="button" style="left: 5vh; top: 5vh;" @click="newCard('testing', 'people')">Create New</div>
            <div class="button" style="right: 5vh; top: 5vh;">Delete</div>
            <div class="topdiv">
                <div v-if="deck && deck.cards.length" v-for="card in deck.cards">
                    <div class="topcarddiv" @click="currentCard = card">
                        {{ card.question.slice(0, 8) }}
                    </div>
                </div>
            </div>
        </nav>
        <div class="scene" @click="flipped = !flipped" v-if="currentCard">
            <div class="card" v-bind:class="{'is-flipped': flipped}">
                <div class="card__face card__face--front">
                    <h1 style="margin: 4vh;">{{ currentCard.question }}</h1>
                </div>
                <div class="card__face card__face--back">
                    <h1 style="margin: 4vh;">{{ currentCard.answer }}</h1>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
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
    .topbar{
        width: 100vw;
        height: 17vh;
        background-color: purple;
        position: fixed;
        top: 0px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .topcarddiv{
        height: 10vh;
        background-color: orange;
        color: purple;
        margin: 0vh 1vw;
        width: 10vh;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 1vh;
        cursor: pointer;
    }
    .topdiv{
        width: 74vw; 
        overflow-x: scroll; 
        display: flex; 
        align-items: center; 
        justify-content: center; 
        height: 17vh;
        border-left: 0.75px solid black;
        border-right: 0.75px solid black;
    }
    ::-webkit-scrollbar{
        width: 0px;
        height: 0px;
    }
    .scene {
        width: 50vw;
        height: 70vh;
        perspective: 3000px;
        margin-top: 17vh;
    }
    .card {
        width: 100%;
        height: 100%;
        position: relative;
        transition: transform 1s;
        transform-style: preserve-3d;
        cursor: pointer;
    }
    .card__face {
        position: absolute;
        height: 100%;
        width: 100%;
        backface-visibility: hidden;
        display: flex;
        flex-wrap: wrap;
    }
    .card__face--front {
        background-color: orange;
        color: purple;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
        border-radius: 2vh;
        border: 4px solid purple;
    }
    .card__face--back {
        background: blue;
        transform: rotateY( 180deg );
        background-color: orange;
        color: purple;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
        border-radius: 2vh;
        border: 4px solid purple;
    }
    .card.is-flipped {
        transform: rotateY(180deg);
    }
</style>