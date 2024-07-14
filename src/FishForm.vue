<script setup>

import { reactive, ref } from "vue";

const props = defineProps({
    fishValue: {type: Object, default:null}
});
const emit = defineEmits(["addFish"]);

const fishURL = ref('');

const form = reactive({
    name: props.fishValue?.name,
});

function saveFish() {
    const data = {
        name: form.name,
        url: fishURL.value
    };

    emit("addFish", data);
}
</script>

<template>
    <div  class="w-full h-full text-white font-bold flex flex-col max-w-[325px]">
        <form @submit.prevent="saveFish">
            <div class="field flex flex-col mb-14">
                <span>Fish Type</span>

                <ul class="flex w-full flex-row flex-wrap mt-4">

                    <li v-for="(fish, index) in fishValue"
                        :key="index"
                        class="w-1/2 focus:outline-none1 focus:ring focus:ring-violet-300"
                        >
                    
                        <label :for="fish.name">
                            <input 
                                class="hidden"
                                type="radio" 
                                :id="fish.name" 
                                :value="fish.url" 
                                v-model="fishURL" 
                            />
                            <img :src="fish.url" :alt="fish.name">
                        </label>
                    </li>
                </ul>
            </div>

            <div class="field flex flex-col max-w-[325px] overflow-hidden">
                <label class="mb-1">
                    Name
                </label>

                <input 
                    type="text" 
                    class="h-12 mb-4 p-2 max-w-[325px] text-black" 
                    placeholder="Enter Fish name"
                    v-model="form.name"
                />

                <button class="p-4 bg-rose-600 w-full">Add Fish</button>
            </div>
        </form>
    </div>

</template>

<style scoped>
    input[type="radio"]:checked + img {
        filter: drop-shadow(-1px 0px 11px #4444dd);
    }
</style>
