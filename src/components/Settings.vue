<script>
    export default {
        name: "Settings",
        methods: {
            returner() {
                this.$emit("return")
            },
            handleDiv(string) {
                if (!this.animations[`${string}Push`]) {
                    this.animations[`${string}Push`] = true;
                    this.animations[`${string}Pull`] = false;
                }
                else {
                    this.animations[`${string}Pull`] = true;
                    this.animations[`${string}Push`] = false;
                }
            },
            changeName() {
                this.handleDiv('change')
                let data = JSON.parse(localStorage.getItem('CardDecks'))
                data.User = this.newName
                this.newName = null
                this.name = data.User
                localStorage.setItem('CardDecks', JSON.stringify(data))
            },
            resetApp() {
                localStorage.removeItem('CardDecks')
                window.location.reload()
            }
        },
        emits: ["return"],
        props: ["user"],
        data() {
            return {
                name: this.user,
                newName: null,
                animations: {
                    changePush: false,
                    changePull: false,
                    resetPush: false,
                    resetPull: false
                }
            }
        }
    }
</script>

<template>
    <div class="backgrounddiv">
        <div class="button" style="right: 5vh; bottom: 5vh;" @click="returner()">
            Return
        </div>
        <h1 style="font-size: 4rem; text-align: center;">Username: {{ name }} <div class="button" style="position: absolute; bottom: 20vh; left: 46.5%;" @click="handleDiv('change')">Change</div></h1>
        <div class="button" style="left: 5vh; bottom: 5vh" @click="handleDiv('reset')">
            Reset App
        </div>
        <div class="creatediv" v-bind:class="{animateUp: animations.changePush, animateDown: animations.changePull}">
            <h1>New Name: <input type="text" placeholder="Enter New Name" style="border-radius: 1vh;" v-model="newName"></h1>
            <div class="button" style="background-color: rgb(189, 0, 189); color: orange; top: 50vh;" @click="() => {
                if (newName){
                    changeName()
                }
                else{
                    handleDiv('change')
                }
            }">Submit</div>
        </div>
        <div class="creatediv" v-bind:class="{animateUp: animations.resetPush, animateDown: animations.resetPull}">
            <h1 style="margin-left: 2vw;">Are you absolutely sure?</h1>
            <h1 style="margin-left: 2vw;">All data will be deleted!</h1>
            <h1>This cannot be undone.</h1>
            <div class="button" style="position: relative; margin: 1vw; background-color: rgb(189, 0, 189); color: orange;" @click="resetApp()">
                Yes
            </div>
            <div class="button" style="position: relative; background-color: rgb(189, 0, 189); color: orange;" @click="handleDiv('reset')">
                No
            </div>
        </div>
        <div style="width: 150vw; height: 150vh; z-index: 2; position: absolute" v-if="animations.changePush" @click="handleDiv('change')"></div>
        <div style="width: 150vw; height: 150vh; z-index: 2; position: absolute; background-color: black; opacity: 0.5;" v-if="animations.resetPush" @click="handleDiv('reset')"></div>
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
    .creatediv{
        position: fixed;
        height: 65vh;
        width: 45vw;
        background-color: orange;
        display: flex;
        justify-content: center;
        align-items: center;
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
</style>