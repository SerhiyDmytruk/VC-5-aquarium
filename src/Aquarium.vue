<script setup>
import { reactive, ref } from "vue";

import FishForm from "./FishForm.vue";
import Fish from "./Fish.vue";

const fishes = ref([
    {
        id: 1,
        url: "./public/golden-purple-fish.png",
        name: 'golden-purple-fish'
    },
    {
        id: 2,
        url: "./public/goldfish.png",
        name: 'goldfish'
    },
    {
        id: 3,
        url: "./public/guppie.png",
        name: 'guppie'
    }, 
    {
        id: 4,
        url: "./public/tropical-fish.png",
        name: 'tropical-fish'
    }, 
    {
        id: 5,
        url: "./public/tuna.png",
        name: 'tuna'
    }
]);

const choosenFishes = ref([]);

function updatedChoosenFishes(data) {
    choosenFishes.value.push(data);
}

</script>

<template>
    <div class="w-full h-full flex flex-row">
        <div class="bg-blue-900 px-8 py-6 max-w-[325px] flex-1">
            <FishForm 
                :fishValue="fishes" 
                @addFish="updatedChoosenFishes"
            />
        </div>
        <div class="bg-blue-900 custom-bg flex-1 relative">
            <Fish 
                v-for="(fish, index) in choosenFishes"
                :fish="fish"
                :key="index"
            />
        </div>
    </div>
</template>

<style scoped>
    .custom-bg {
        background-image: url("./public/bg.jpg");
        background-repeat: no-repeat;
        background-size: 100% 100%;
    }
</style>
