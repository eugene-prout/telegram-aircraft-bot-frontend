<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
const props = defineProps<{
    message: string
    symbol: string
    interval: number
    count: number
}>();

const timer = ref();
const counter = ref(0);
const display = ref(props.message)

function timerTick() {
    console.log("Running countDownFunc")
    if (counter.value < props.count) {
        counter.value++
        display.value += props.symbol
    }
    else {
        counter.value = 1
        display.value = props.message + props.symbol
    }
}


onMounted(() => {
    timer.value = setInterval(() => {
        timerTick();
    }, props.interval);
});

onBeforeUnmount(() => {
    timer.value = null;
});

</script>

<template>
    <span>
        {{ display }}
    </span>
</template>