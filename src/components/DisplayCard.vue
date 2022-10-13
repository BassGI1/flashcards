<script>
    export default {
        name: "DisplayCard",
        props: ["text"],
        emits: ["tabchange"],
        methods: {
            handleClick() {
                this.$emit('tabchange')
            }
        }
    }
</script>

<template>
    <div class="displaycard" @click="handleClick()">
        <h1 style="font-size: 3rem;">{{ text }}</h1>
    </div>
</template>

<style scoped>
    .displaycard{
        width: 40%;
        height: 40%;
        background-color: rgb(189, 0, 189);
        border: .7vh solid purple;
        border-radius: 3vh;
        cursor: pointer;
        margin: 2vh 5vh;
        color: orange;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>