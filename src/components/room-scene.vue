<template>
    <div class="container">
        <div class="header">
            <h1>Hi Nitya</h1>
            <h1>This is your virtual room</h1>
            <p>(more changes will come)</p>
        </div>

        <div class="gifts">
            <div class="gift" :class="{ 'opened': selected === i }" @click="selected = g"
                v-for="(g, i) in Object.keys(data)" :key="i">
                <span style="font-weight: bold;">{{ g }}th</span> Feb
            </div>
        </div>

        <div class="display" v-if="selected" @click="selected = null">
            <h3>Here are your gifts</h3>
            <div class="items">
                <div class="item" v-for="item in selectedData.gifts" :key="item">
                    <img :src="'/valde/' + item" alt="">
                </div>
            </div>
            <div class="contents" v-html="selectedData.letter">
            </div>
        </div>
    </div>
</template>

<style scoped>
.container {
    color: white;
    height: 100vh;
    background: linear-gradient(rgba(255, 65, 65, 0.884), rgb(161, 0, 67));
}

.header {
    padding: 1rem;
    max-width: 50rem;
    margin: auto;

    h3 {
        margin-top: 0;
    }
}

.gifts {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap: 1rem;
    place-items: center;
    padding: 1rem;
    max-width: 50rem;
    margin: auto;
    margin-top: 10rem;

    .gift {
        height: 4rem;
        width: 4rem;
        padding: 1rem;
        background-color: wheat;
        color: black;
        border: 3px solid goldenrod;
        border-radius: 1rem;
        font-size: 120%;
        text-align: center;
        transition: 0.3s all ease-out;
        box-shadow: 1px 1px 1px 3px rgba(0, 0, 0, 0.459);
        cursor: pointer;

        &:hover {
            box-shadow: 2px 2px 3px 4px rgba(0, 0, 0, 0.425);
            scale: 1.1;
        }
    }
}

.display {
    position: absolute;
    inset: 0;
    height: 100vh;
    background-color: #000a;
    padding: 3rem;

    .items {
        overflow-x: auto;
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        scroll-behavior: smooth;
        flex-shrink: 0;
        overflow: auto;
        height: calc(35vh);
        background-color: rgba(255, 255, 255, 0.171);
        gap: 1rem;
        padding: 1rem;
        box-sizing: border-box;

        .item {
            flex-shrink: 0;
        }

        img {
            height: 30vh;
            width: auto;
        }
    }

    .contents {
        background-color: wheat;
        margin-top: 3rem;
        color: black;
        padding: 3rem;
        height: 30vh;
        overflow: auto;
    }
}
</style>

<script setup>
import { computed, ref } from 'vue';
import data from '../data.json';
const selected = ref(0);
const selectedData = computed(() => {
    if (!selected) return null;
    return data[selected.value.toString()] ?? null;
})

</script>