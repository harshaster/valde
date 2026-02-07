<template>
    <div class="container">
        <video autoplay muted @ended="videoEnded">
            <source src="../assets/nhome.mp4">
        </video>
        <div class="overlay" v-if="showOverlay">
            Your order has arrived. Get ready to meet our delivery partner.
            <button @click="accept">Accept it</button>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['accept'])

const showOverlay = ref(false);
const videoEnded = () => {
    showOverlay.value = true;
}
const accept = () => {
    emit('accept');
}
</script>

<style scoped>
.container {
    font-family: "Indie Flower", cursive;
    height: 100vh;
    overflow: hidden;
    display: flex;
    justify-content: center;
}

.overlay {
    position: absolute;
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: end;
    text-align: center;
    font-size: 110%;
    gap: 1.5rem;
    align-items: center;
    padding: 1rem;
    inset: 0;
    background: linear-gradient(to top, black, transparent);
    animation: fade-in 0.7s linear;

    button {
        font-family: inherit;
        color: inherit;
        background-color: transparent;
        outline: none;
        border: none;
        border: 1px solid white;
        padding: 0.5rem 1rem;
    }
}

@keyframes fade-in {
    0% {
        opacity: 0;
    }

    50% {
        opacity: 0.25;
    }

    100% {
        opacity: 1;
    }
}
</style>