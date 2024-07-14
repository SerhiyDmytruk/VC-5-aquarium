<script setup>
import { ref, onUnmounted,onMounted, onBeforeUnmount } from 'vue';

const props = defineProps({
  fish: { type: Object, default: null },
});

let progressValue = ref(0);
let timeleft = ref(10);

const interval = setInterval(function(){
  if(timeleft.value <= 0) clearInterval(interval);
  
  progressValue.value = 10 - timeleft.value;
  timeleft.value -= 1;
}, 1000);

onUnmounted(() => clearInterval(interval));

const resetTimer = function () {
    timeleft.value = 10;
}


const animatedElement = ref(null);
let position = { x: 0, y: 0 };
let direction = { x: 1, y: 1 };
let animationInterval;

const getRandomDirection = () => {
  return Math.random() < 0.5 ? -1 : 1;
};

const animateElement = () => {
  position.x += direction.x * 10;
  position.y += direction.y * 10;

  const element = animatedElement.value;
  const parent = element.parentElement;

  if (position.x <= 0 || position.x + element.offsetWidth >= parent.clientWidth) {
    direction.x = getRandomDirection();
  }
  if (position.y <= 0 || position.y + element.offsetHeight >= parent.clientHeight) {
    direction.y = getRandomDirection();
  }

  element.style.transform = `translate(${position.x}px, ${position.y}px)`;
};

onMounted(() => {
  animationInterval = setInterval(animateElement, 1000);
});

onBeforeUnmount(() => {
  clearInterval(animationInterval);
});
</script>

<template>
    <div 
        ref="animatedElement"
        class="fish animated-element"
        :class="{hidden: timeleft < 0 }"
        @click="resetTimer"
        >

        <div v-if="timeleft < 5" class="info">
            Feed me!
        </div>

        <img v-if="timeleft > 2" :src="fish.url" />
        <img v-else src="/public/dead.png" />

        <div class=" name">
            <p>{{fish.name}}
                <progress :value="progressValue" max="10" class="progress"></progress>
            </p>
        </div>
    </div>
</template>

<style scoped>

.fish {
    @apply w-36 absolute top-0 left-0 flex justify-center items-center flex-col;
}

.fish .info {
    @apply bg-cyan-400/[.8] rounded-sm text-center text-sm px-2 text-nowrap;
}

.fish .name {
    @apply bg-zinc-700/[.8] text-white text-center;
}

.fish .progress {
    height: 5px;
    transition: .500s;
}

</style>
