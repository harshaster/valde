<script setup>
import { onMounted, ref, watch } from 'vue';

const emit = defineEmits(['gotoRoom'])

const nameInput = ref('');
const chatState = ref(0);
let intervalInstance;

onMounted(() => {
    intervalInstance = setInterval(() => {
        incrementChat();
    }, 3000);
});

const incrementChat = () => {
    chatState.value++;
}

watch(nameInput, (inp) => {
    if (inp.toLowerCase() === 'nitya') {
        chatState.value++;
    }
})

watch(chatState, (inp) => {
    if (inp % 2 === 0) {
        intervalInstance = setInterval(() => {
            chatState.value++;
        }, 3000);
    } else {
        clearInterval(intervalInstance);
    }

    if (inp === 6) {
        clearInterval(intervalInstance)
    }
})
</script>

<template>
    <div class="cont">
        <div class="bubbles" v-if="chatState < 6" :style="{ left: chatState % 2 === 0 ? '1rem' : '10rem' }">
            <div class="bubble">
                <span v-if="chatState === 0">Ma'am, kya naam se order hai?</span>
                <label v-if="chatState === 1" for="name">Your name:</label>
                <input v-if="chatState === 1" type="text" name="name" v-model="nameInput" />
                <span v-else-if="chatState === 2">Thank You ma'am. Ye lijiye aapka order</span>
                <button @click="incrementChat" v-else-if="chatState === 3">Thank You</button>
                <span v-else-if="chatState === 4">Arey ma'am ye letter bhi hai!</span>
                <span v-else-if="chatState === 5"><button @click="incrementChat">Oh, thank you</button></span>
            </div>
            <i v-if="chatState === 3 || chatState == 5">your items will be present in the room afterwards</i>
        </div>

        <button class="out" v-else @click="emit('gotoRoom')">Go to room</button>

        <img src="../assets/arrived.png" alt="" v-if="[0, 1].includes(chatState)">
        <img src="../assets/box.png" alt="" v-if="chatState === 3">
        <img src="../assets/scroll-big.png" alt="" v-if="chatState === 5">
        <img src="../assets/giving.png" alt="" v-if="chatState === 2">
        <img src="../assets/scroll.png" alt="" v-if="chatState === 4">
        <img src="../assets/bye.png" alt="" v-if="chatState === 6">
    </div>
</template>

<style scoped>
.cont {
    height: 100vh;
    background: linear-gradient(rgba(255, 255, 255, 0.685), rgba(255, 255, 255, 0.932)), url(../assets/nearhome.png);
    background-position: 50% 20%;
    position: relative;

    .bubbles {
        position: absolute;
        top: 9rem;

        .bubble {
            background-color: white;
            border: 1px solid black;
            padding: 3rem 2rem;
            border-radius: 50%;
            max-width: 10rem;
            margin: auto;
        }
    }

    button.out {
        position: absolute;
        top: 10rem;
        left: 30%;
        color: black;
        font-size: larger;
        background-color: white;
        border: 1px solid black;
        padding: 0.5rem 1rem;
        border-radius: 0.5rem;
    }



    img {
        position: absolute;
        bottom: 5rem;
        width: 100%;
        max-width: 30rem;

        @media (min-width:1000px) {
            left: 40%;
        }
    }
}
</style>