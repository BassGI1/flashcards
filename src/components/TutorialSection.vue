<script>
    export default {
        name: "TutorialSection",
        props: ["Image"],
        emits: ["Return"],
        methods: {
            handleReturn() {
                this.$emit("Return")
            }
        }
    }
</script>

<template>
    <div class="quarter">
        <img :src="Image" alt="Chadku Chadding" style="height: 80%;">
        <h3 style="width: 100%; text-align: center;">Goku do be Chadding lol</h3>
    </div>
</template>

<style>
    .quarter{
        width: 50%;
        height: 50%;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        justify-content: center;
    }
</style>