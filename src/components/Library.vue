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
                decks: []
            }
        },
        mounted() {
            this.data.forEach(x => {
                this.decks.push(x)
            })
        },
        methods: {
            handleCreate() {
                
            }
        }
    }
</script>

<template>
    <div class="backgrounddiv">
        <div class="button" style="left: 2vh; top: 2vh;">Create</div>
        <div v-for="deck in decks">
            {{deck.name}}
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
</style>