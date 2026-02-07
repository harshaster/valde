<script setup lang="ts">
import { onMounted, ref } from 'vue';
import CallInterface from './components/call-interface.vue';
import MainScene from './components/main-scene.vue';
import Stage3 from './components/stage-3.vue';
import RoomScene from './components/room-scene.vue';

const state = ref(1);
const timerCount = 10;
const timer = ref(timerCount);
let timerInstance;
const onCallCut = () => {
	state.value = 3;
}

onMounted(() => {
	startTimer()
});

const onAcceptOrder = () => {
	state.value = 2;
}
const onRoom = () => {
	state.value = 4;
}
const startTimer = () => {
	timerInstance = setInterval(() => {
		timer.value--;
		if (timer.value === 0) {
			clearInterval(timerInstance);
			state.value = 1;
		}
	}, 1000);
}
</script>

<template>
	<main>
		<div class="stage-2" v-if="state === 0">
			<img src="./assets/riding.png" />
			<p>Your order is coming soon...</p>
			<div class="timer">
				<span>Arriving in</span>
				<br>
				<span style="font-size: 80px;">
					{{ timer }}
				</span>s
			</div>
		</div>
		<MainScene v-else-if="state === 1" @accept="onAcceptOrder" />
		<CallInterface @call-cut="onCallCut" v-if="state === 2" />
		<Stage3 v-else-if="state === 3" @gotoRoom="onRoom" />
		<RoomScene v-else-if="state === 4" />
	</main>
</template>

<style scoped>
main {
	font-family: "Indie Flower", cursive;
}

.stage-2 {
	background-color: white;
	color: black;
	display: flex;
	flex-direction: column;
	align-items: center;
	height: 100vh;
	justify-content: center;

	img {
		width: 100%;
	}

}
</style>