<script>
    export default {
        name: "TestMode",
        props: ["data"],
        data() {
            return {
                deck: [],
                currentCard: null,
                flipped: false,
                started: false,
                loadingballs: {
                    '2': false,
                    '3': false,
                    '4': false,
                    '5': false
                },
                interval: 'null',
                changeCard: false
            }
        },
        mounted() {
            if (this.data.length){
                this.data.forEach(x => this.deck.push(x))
                this.shuffle()
                this.currentCard = this.deck[0]
            }
            setTimeout(() => {this.loadingballs['2'] = true}, 300)
            setTimeout(() => {this.loadingballs['3'] = true}, 600)
            setTimeout(() => {this.loadingballs['4'] = true}, 900)
            setTimeout(() => {this.loadingballs['5'] = true}, 1200)
            setTimeout(() => {
                this.started = true
                this.interval1 = setInterval(() => {
                    this.flipped = !this.flipped
                    let curr = this.deck.filter(x => x.id == this.currentCard.id)[0]
                    if (this.deck.indexOf(curr) == this.deck.length - 1){
                        clearInterval(this.interval1)
                    }
                    else if (this.changeCard){
                        this.changeCard = false
                        setTimeout(() => this.currentCard = this.deck[this.deck.indexOf(curr) + 1], 1000)
                    }
                    else{
                        this.changeCard = true
                    }
                }, 10000)
            }, 5000)
        },
        methods: {
            shuffle() {
                let m = this.deck.length, t, i;
                while (m) {
                    i = Math.floor(Math.random() * m--);
                    t = this.deck[m];
                    this.deck[m] = this.deck[i];
                    this.deck[i] = t;
                }
            }
        }
    }
</script>

<template>
    <div class="backgrounddiv">
        <div class="loadingdiv" v-if="!started">
            <div class="loadingball load" style="margin-right: 6.25%;"></div>
            <div class="loadingball" v-bind:class="{load: loadingballs['2']}" style="margin-right: 6.25%;"></div>
            <div class="loadingball" v-bind:class="{load: loadingballs['3']}" style="margin-right: 6.25%;"></div>
            <div class="loadingball" v-bind:class="{load: loadingballs['4']}" style="margin-right: 6.25%;"></div>
            <div class="loadingball" v-bind:class="{load: loadingballs['5']}"></div>
        </div>
        <h1 style="position: absolute; top: -0.5vh; font-size: 3rem;" v-if="!started">Test Mode <span style="font-size: 1rem;">- Get Ready!</span></h1>
        <div class="scene" v-if="currentCard && started" >
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
        z-index: 1;
    }
    .scene {
        width: 50vw;
        height: 70vh;
        perspective: 3000px;
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
    .loadingdiv{
        width: 55vw;
        height: 15vh;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
    }
    .loadingball{
        width: 6vw;
        height: 6vw;
        background-color: orange;
        position: relative;
        border-radius: 50vh;
    }
    @keyframes load{
        50% {
            width: 8vw;
            height: 8vw;
            background-color: purple;
        }
        100% {
            width: 6vw;
            height: 6vw;
            background-color: orange;
        }
    }
    .load{
        animation: load 1.5s infinite;
    }
</style>