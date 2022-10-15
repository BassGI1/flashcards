<script>
    export default {
        name: "CardView",
        props: ["data"],
        data() {
            return {
                deck: null
            }
        },
        mounted() {
            this.deck = {...this.data}
            console.log(this.deck)
        }
    }
</script>

<template>
    <div class="backgrounddiv">
        darkie
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
</style>