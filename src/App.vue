<script setup lang="ts">
import { ref, reactive } from 'vue';

const breakTime = ref(0);
const isRunning = ref(false);
const timeLeft = ref(0);
const history = reactive<number[]>([]);
const round = ref(0);
let interval: null | number = null;
const start = () => {
  round.value++;
  isRunning.value = true;

  timeLeft.value = breakTime.value;

  interval = setInterval(() => {
    if (timeLeft.value === 0) {
      stop();
    } else {
      timeLeft.value--;
    }
  }, 1000);
};

const stop = () => {
  isRunning.value = false;
  history.push(round.value);
  if (interval != null) {
    clearInterval(interval);
  }
};
</script>

<template>
  <div class="container">
    <h1 v-if="isRunning" :class="{ title: timeLeft <= 10 }">{{ timeLeft }}</h1>
    <label for="time-input"></label>
    <input
      v-model.number="breakTime"
      type="number"
      name="time-input"
      id="time-input"
    />
    <button @click="start">Start</button>
    <br />
    <ul>
      <li v-for="entry in history" :key="entry">Round {{ entry }}</li>
    </ul>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  width: 300px;
  align-items: center;
  justify-content: center;
}

.title {
  animation: bounce infinite 1s;
  color: red;
}

@keyframes bounce {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>
