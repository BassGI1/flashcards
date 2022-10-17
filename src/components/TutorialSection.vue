<script>
    export default {
        name: "TutorialSection",
        props: ["Image", "Text"],
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
        <img :src="Image" alt="Chadku Chadding" style="height: 80%; border: 3px solid purple; border-radius: 2vh;">
        <h3 style="width: 100%; text-align: center;">{{Text}}</h3>
    </div>
</template>

<style scoped>
    .quarter{
        width: 50%;
        height: 50%;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        justify-content: center;
        margin-top: 1vh;
    }
</style>