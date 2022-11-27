<script>
    export default {
        name: "About",
        methods: {
            returner() {
                this.$emit("return")
            }
        },
        emits: ["return"],
        mounted() {
            setTimeout(() => this.animate = '1', 6000)
            setTimeout(() => this.animate = '2', 10000)
            setTimeout(() => this.animate = '3', 14000)
        },
        data() {
            return {
                animate: false
            }
        }
    }
</script>

<template>
    <div class="backgrounddiv" :style="{background: animate == '3' ? 'center/100% url(/background_1.jpg)' : ''}">
        <div class="button" style="right: 5vh; bottom: 5vh;" @click="returner()">
            Return
        </div>
        <h1>Flash Cards V1.1, built entirely by Bassam El-Naggar</h1>
        <h4 style="position: absolute; bottom: 20vh" v-if="animate">{{ animate == '1' ? 'This page is a little empty . . .' : animate == '2' ? "Let's spice it up a little" : "That's better" }}</h4>
        <img src="/confusion.gif" alt="confusion" class="confusion" v-if="animate">
    </div>
</template>

<style scoped>
    .backgrounddiv{
        background-color: rgb(189, 0, 189);
        position: absolute;
        left: 0px;
        top: 0px;
        display: flex;
        flex-wrap: wrap;
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
    .confusion{
        height: 20vh;
        position: absolute;
        bottom: 0px;
    }
</style>