<template>
  <div class="releaxer-app">
    <h1>Relaxer</h1>
    <div class="container" id="container" :class="{'grow': growState, 'shrink': shrinkState}">
      <div class="circle"></div>
      <p id="text"> {{ textData }}</p>
      <div class="pointer-container">
        <span class="pointer"></span>
      </div>
      <div class="gradient-circle"></div>
    </div>
  </div>
</template>

<script setup>
useSeoMeta({
  title: 'Relaxer App',
  description: 'Relaxer App',
  keywords: 'relaxer, app, vue, vite, nuxt,',
  twitterTitle: 'Relaxer App',
  twitterDescription: 'Relaxer App',
  twitterCard: 'summary',
  ogDescription: 'Relaxer App',
  ogTitle: 'Relaxer App',
  ogType: 'website',
})
const textData = ref('Breathe In!');
const totalTime = 7500;
const breatheTime = (totalTime / 5) * 2;
const holdTime = totalTime / 5;

const growState = ref(false);
const shrinkState = ref(false);

let breatheIntervalId;

function breatheAnimation() {
  textData.value = 'Breathe In!';
  growState.value = true;
  setTimeout(() => {
    textData.value = 'Hold';

    setTimeout(() => {
      textData.value = 'Breathe Out!';
      growState.value = false;
      shrinkState.value = true;
    }, holdTime);
  }, breatheTime);
}

onMounted(() => {
  breatheAnimation();
  breatheIntervalId = setInterval(breatheAnimation, totalTime);
});

onBeforeUnmount(() => {
  clearInterval(breatheIntervalId);
});
</script>

<style scoped>

.releaxer-app {
  background: #224941 url('./img/bg.jpg') no-repeat center center/cover;
  color: #fff;
  font-family: 'Montserrat', sans-serif;
  min-height: 100vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
}

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: auto;
  height: 300px;
  width: 300px;
  position: relative;
  transform: scale(1);
}

.circle {
  background-color: #010f1c;
  height: 100%;
  width: 100%;
  border-radius: 50%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
}

.gradient-circle {
  background: conic-gradient(
    #55b7a4 0%,
    #4ca493 40%,
    #fff 40%,
    #fff 60%,
    #336d62 60%,
    #2a5b52 100%
  );
  height: 320px;
  width: 320px;
  z-index: -2;
  border-radius: 50%;
  position: absolute;
  top: -10px;
  left: -10px;
}

.pointer {
  background-color: #fff;
  border-radius: 50%;
  height: 20px;
  width: 20px;
  display: block;
}

.pointer-container {
  position: absolute;
  top: -40px;
  left: 140px;
  width: 20px;
  height: 190px;
  animation: rotate 7.5s linear forwards infinite;
  transform-origin: bottom center;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}

.container.grow {
  animation: grow 3s linear forwards;
}

@keyframes grow {
  from {
    transform: scale(1);
  }

  to {
    transform: scale(1.2);
  }
}

.container.shrink {
  animation: shrink 3s linear forwards;
}

@keyframes shrink {
  from {
    transform: scale(1.2);
  }

  to {
    transform: scale(1);
  }
}
</style>