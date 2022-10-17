<script>
import TestMode from './TestMode.vue'
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
            flipped: false,
            renderNewCardDiv: false,
            renderDeleteCardDiv: false,
            renderDivData: false,
            question: null,
            answer: null,
            saved: false,
            renderDeleteDeckDiv: false,
            testmode: false
        };
    },
    mounted() {
        this.deck = { ...this.data };
    },
    methods: {
        handleCreateClick() {
            this.renderNewCardDiv = true;
            setTimeout(() => { this.renderDivData = true; }, 1000);
        },
        handleCreate(q, a) {
            this.deck.cards.push(new Card(q, a, this.deck.cards.length + 1));
            this.question = null;
            this.answer = null;
            this.renderNewCardDiv = false;
            this.renderDivData = false;
            this.flipped = false;
            this.renderDeleteCardDiv = false;
        },
        handleCancel() {
            this.question = null;
            this.answer = null;
            this.renderNewCardDiv = false;
            this.renderDivData = false;
            this.renderDeleteCardDiv = false;
        },
        handleDeleteClick() {
            this.renderDeleteCardDiv = true;
            setTimeout(() => { this.renderDivData = true; }, 1000);
        },
        handleDelete() {
            let temp = this.deck.cards.filter(card => card.id != this.currentCard.id);
            temp.forEach((x, i) => {
                x.id = i + 1;
            });
            this.deck.cards = temp;
            this.currentCard = this.deck.cards[0] || null;
            this.renderDivData = false;
            this.renderDeleteCardDiv = false;
        },
        save() {
            this.saved = true;
            setTimeout(() => {
                this.saved = false;
            }, 2000);
            this.$emit("save", this.deck);
        },
        returner() {
            this.$emit("return");
        },
        deleter() {
            this.save();
            this.$emit("delete", this.deck);
        }
    },
    emits: ["save", "return", "delete"],
    components: { TestMode }
}
</script>

<template>
    <div class="backgrounddiv">
        <nav class="topbar" v-if="!testmode">
            <div class="button" style="left: 5vh; top: 5vh;" @click="handleCreateClick()">Create New</div>
            <div class="button" style="right: 5vh; top: 5vh;" @click="handleDeleteClick()">Delete</div>
            <div class="topdiv">
                <div v-if="deck && deck.cards.length" v-for="card in deck.cards">
                    <div class="topcarddiv" @click="currentCard = card">
                        {{ card.question.slice(0, 8) }}
                    </div>
                </div>
            </div>
        </nav>
        <div class="scene" @click="flipped = !flipped" v-if="currentCard && !testmode" >
            <div class="card" v-bind:class="{'is-flipped': flipped}">
                <div class="card__face card__face--front">
                    <h1 style="margin: 4vh;">{{ currentCard.question }}</h1>
                </div>
                <div class="card__face card__face--back">
                    <h1 style="margin: 4vh;">{{ currentCard.answer }}</h1>
                </div>
            </div>
        </div>
        <h1 v-if="!deck || !deck.cards.length && !testmode">There's nothing here . . .</h1>
        <div v-if="renderNewCardDiv && !testmode" class="newcarddiv">
                <h3 v-if="renderDivData" style="width: 100%;"><span style="margin-left: 3vw;">Question: <input type="text" placeholder="Question" v-model="question" style="margin-right: 3vw; border-radius: 1vh;"> Answer: <input type="text" placeholder="Answer" v-model="answer" style="border-radius: 1vh;"></span></h3>
                <div class="button" style="position: relative; margin-right: 2vw;" v-if="renderDivData" @click="handleCancel()">
                    Cancel
                </div>
                <div class="button" v-if="renderDivData" style="position: relative; margin-left: 2vw;" @click="() => {
                    if (question){
                        handleCreate(question, answer)
                    }
                }">
                    Create
                </div>
        </div>
        <div class="toggle" @click="testmode = !testmode">
            <div class="toggleball" v-bind:class="{moveright: testmode, moveleft: !testmode}"></div>
        </div>
        <div v-if="renderDeleteCardDiv && !testmode" class="deletecarddiv">
                <h3 style="margin-right: 2vw; margin-top: -4vh;" v-if="renderDivData">Are you sure you want to delete this card?</h3>
                <div class="button" style="position: relative; margin-right: 2vw; margin-top: -8vh;" v-if="renderDivData" @click="handleDelete()">
                    Yes
                </div>
                <div class="button" v-if="renderDivData" style="position: relative; margin-left: 2vw; margin-top: -8vh;" @click="handleCancel()">
                    No
                </div>
                <div class="button" style="bottom: 3vh;" @click="renderDeleteDeckDiv = true" v-if="renderDivData">
                    Delete Deck
                </div>
        </div>
        <div class="button" style="right: 5vh; bottom: 5vh;" @click="save()" v-if="!testmode">
            Save
        </div>
        <div class="button" style="left: 5vh; bottom: 5vh;" @click="returner()" v-if="!testmode">
            Return
        </div>
        <div v-if="renderDeleteDeckDiv && !testmode" style="position: absolute; width: 100vw; height: 100vh; background-color: black; opacity: 0.5; display: flex; align-items: center; justify-content: center; z-index: 5;">
        </div>
        <div v-if="renderDeleteDeckDiv" style="width: 60vh; height: 60vh; background-color: orange; position: absolute; border-radius: 2vh; color: purple; display: flex;
         align-items: center; justify-content: center; flex-wrap: wrap; z-index: 6;">
            <h3 style="margin-bottom: -15vh;">Are you sure you want to delete this Deck?</h3>
            <div class="button" style="background-color: rgb(175, 122, 225); position: relative; margin-right: 1vw;" @click="deleter()">Yes</div>
            <div class="button" style="background-color: rgb(175, 122, 225); position: relative; margin-left: 1vw;" @click="() => {
                renderDeleteDeckDiv = false
                renderDeleteCardDiv = false
                renderDivData = false
            }">No</div>
        </div>
        <h1 v-if="saved" class="save">Saved!</h1>
    </div>
    <TestMode v-if="testmode" :data="deck.cards"/>
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
    .newcarddiv{
        position: absolute;
        left: 0px;
        top: 17vh;
        width: 13vw;
        height: 0px;
        background-color: purple;
        color: orange;
        animation: grow 1s forwards;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
        border-radius: 0px 0px 2vh 0px;
        border-bottom: 2px solid black;
        border-right: 2px solid black;
    }
    @keyframes grow{
        100%{
            height: 30vh;
            width: 50vw;
        }
    }
    .deletecarddiv{
        position: absolute;
        right: 0px;
        top: 17vh;
        width: 13vw;
        height: 0px;
        background-color: purple;
        color: orange;
        animation: grow 1s forwards;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
        border-radius: 0px 0px 0px 2vh;
        border-bottom: 2px solid black;
        border-left: 2px solid black;
    }
    .save{
        position: absolute;
        font-size: 5rem;
        color: black;
        animation: save 2s forwards;
    }
    @keyframes save{
        100%{opacity: 0;}
    }
    .toggle{
        overflow: hidden;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 11vh;
        height: 3.5vh;
        position: absolute;
        bottom: 1vh;
        background-color: rgb(168, 168, 168);
        border-radius: 50vh;
        cursor: pointer;
        border: 1px solid purple;
        z-index: 2;
    }
    .toggleball{
        width: 3.5vh;
        height: 100%;
        background-color: orange;
        border-radius: 50vh;
        position: relative;
        transform: translateX(-3.75vh);
    }
    .moveright{
        animation: moveright 1s forwards;
    }
    .moveleft{
        animation: moveleft 1s forwards;
    }
    @keyframes moveright{
        0% {transform: translateX(-3.75vh);}
        100% {transform: translateX(3.75vh);}
    }
    @keyframes moveleft{
        0% {transform: translateX(3.75vh);}
        100% {transform: translateX(-3.75vh);}
    }
</style>