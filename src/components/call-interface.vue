<script setup>
import { computed, ref, useTemplateRef, watch } from 'vue';

const emits = defineEmits(['callCut']);

const callIcon = useTemplateRef('callIcon');
const sliding = ref(false);
const left = ref(0);
const answered = ref(false);

const sound = new Audio('/crossing_nothing.mp3');
const resp = new Audio('/answer.m4a');
sound.volume = 0.7;
sound.play();

const nextLeft = computed(() => Math.exp(Math.E, left.value * 0.02));

const cutCall = () => {
    answered.value = false;
    emits('callCut');
}

const slideCall = (e) => {
    e.preventDefault();
    sliding.value = true;
}
const onMove = (ev) => {
    if (!sliding.value) {
        return;
    }
    ev.offsetX > 0 ? left.value += nextLeft.value : left.value -= nextLeft.value;
    if (Math.abs(left.value) > 109) {
        return;
    }
    callIcon.value.style.transform = 'translateX(' + (left.value) + 'px)';
}

const onUp = () => {
    sliding.value = false;
    if (left.value < 109) {
        left.value = 0;
        callIcon.value.style.transform = 'translateX(' + (left.value) + 'px)';
        return;
    }
    left.value = 0;
    answered.value = true;
}

watch(answered, (val) => {
    if (val) {
        sound.pause();
        resp.play();
    }
})

</script>

<template>
    <div class="container">
        <div class="top-bar">
            <div class="left">
                <span>12:30 PM</span>
                <span>
                    <i class="ri-phone-fill"></i> Phone
                </span>
            </div>
            <div class="right">
                <i class="ri-signal-cellular-3-fill"></i>
                <i class="ri-signal-wifi-2-fill"></i>
                <i class="ri-battery-low-line"></i>
            </div>
        </div>

        <div class="center">
            Delivery for you
            <div class="sub-text">Mobile &middot; +91 9565129976</div>
            <div class="icon">
                <img src="../assets/peep-standing.svg" alt="">
            </div>
        </div>

        <div class="bottom">
            <div class="answer" v-if="!answered">
                <span>Decline</span>
                <span ref="callIcon" style="touch-action:none; user-select:none; display:inline-block;"
                    @pointerdown="slideCall($event)" @pointerup="onUp($event)" @pointermove="onMove($event)">
                    <i class="ri-phone-fill"></i>
                </span>
                <span>Answer</span>
            </div>
            <div class="answered" v-else>
                <div class="tools">
                    <i class="ri-keyboard-fill"></i>
                    <i class="ri-mic-fill"></i>
                    <i class="ri-volume-up-fill"></i>
                    <i class="ri-more-2-fill"></i>
                </div>
                <div class="end-call" @click="cutCall">
                    <i class="ri-phone-fill"></i>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap');

.container {
    padding: 10px;
    font-family: "Open Sans", sans-serif;
    background-color: #F1F4F9;
    height: 100vh;
}

.top-bar {
    font-size: 90%;
    display: flex;
    justify-content: space-between;

    .left {
        display: flex;
        gap: 0.8rem;
    }

    .right {
        display: flex;
        gap: 0.5rem;
    }
}

.center {
    margin-top: 7rem;
    font-weight: 600;
    color: #1f1f1f;
    font-size: 3rem;
    display: flex;
    align-items: center;
    flex-direction: column;
    gap: 1rem;

    .sub-text {
        font-size: 20px;
        font-weight: 500;
        opacity: 60%;
    }

    .icon {
        margin-top: 2rem;
        height: 15rem;
        width: 15rem;
        box-sizing: border-box;
        overflow: hidden;
        border-radius: 50%;
        background-color: white;
        box-shadow: 2px 2px 5px 5px #b2b8be67;
        position: relative;
        transition: all 0.1s;

        img {
            position: absolute;
            left: 10px;
            top: 20px;
            transform: scale(1.5);
            transform-origin: top;
        }
    }
}

.bottom {
    display: flex;
    justify-content: center;
    margin-top: 15rem;

    .answer {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: #DFE2E7;
        width: 80%;
        padding: 10px 20px;
        box-sizing: border-box;
        border-radius: 3rem;
        transition: 0.1s linear;
    }

    span:first-child,
    span:nth-child(3) {
        font-size: 90%;
        font-weight: 500;
    }

    span:nth-child(2) {
        color: rgb(0, 151, 0);
        text-align: center;
        padding: 0.5rem 1.5rem;
        background-color: white;
        border-radius: 2rem;
        font-size: 200%;

        i {
            animation: tilt-shaking 0.2s infinite;
            display: inline-block;
        }
    }

    .answered {
        margin-top: -3rem;
        height: 13rem;
        width: 100%;
        border-radius: 1rem 1rem 0 0;
        background-color: #DFE2E7;

        .tools {
            display: flex;
            justify-content: space-between;
            padding: 1rem;

            i {
                background-color: white;
                font-size: 1.5rem;
                padding: 15px 25px;
                border-radius: 3rem;
            }
        }

        .end-call {
            color: white;
            background-color: rgb(196, 0, 0);
            width: 80%;
            margin: auto;
            font-size: 2rem;
            text-align: center;
            margin-top: 1rem;
            padding: 0.5rem 0;
            border-radius: 3rem;
        }
    }
}

@keyframes tilt-shaking {
    0% {
        transform: rotate(0deg);
    }

    25% {
        transform: rotate(5deg);
    }

    50% {
        transform: rotate(0eg);
    }

    75% {
        transform: rotate(-5deg);
    }

    100% {
        transform: rotate(0deg);
    }
}
</style>
