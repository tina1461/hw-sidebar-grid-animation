<template>
  <div class="grid-container">
    <div v-for="(cell, index) in 9" :key="index" class="grid-cell" :class="{ 'flash': shouldFlash(index) }">
      <div v-if="isCorner(index)" class="ball" :style="ballPosition(index)"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GridAnimation',
  methods: {
    shouldFlash(index) {
      return [1, 5, 7].includes(index)
    },
    isCorner(index) {
      return [0, 2, 6, 8].includes(index)
    },
    ballPosition() {
      return {
        transform: 'translateX(50px)',
      }
    }
  }
}
</script>

<style>
.grid-container {
  padding: 20px;
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  justify-items: center;
  align-items: center;
}

.grid-cell {
  width: 100px;
  height: 100px;
  border: 2px solid black;
  background: radial-gradient(circle, rgba(113,81,95,1) 81%, rgba(0,0,0,1) 100%);
  position: relative;
}

.flash {
  animation: flashAnimation 1s infinite;
}

.ball {
  width: 30px;
  height: 30px;
  background-color: #A5F12B;
  position: absolute;
  top: calc(50% - 15px);
  left: calc(50% - 60px);
  border-radius: 50%;
  animation: moveRight 2s linear infinite;
  z-index: 10;
}

@keyframes flashAnimation {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}

@keyframes moveRight {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(300px);
  }
}
</style>
