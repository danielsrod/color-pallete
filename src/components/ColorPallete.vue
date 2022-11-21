<script setup>
import { onMounted, ref } from 'vue';

const colors = ref(['', '', '', '', '']);

const generateRandomHex = () => {
  colors.value = colors.value.map((key) => {
    return '#' + Math.floor(Math.random() * 16777215).toString(16);
  });
};

onMounted(() => {
  generateRandomHex();
  addEventListener('keypress', function (e) {
    if (e.code === 'Space') {
      generateRandomHex();
    }
  });
});
</script>

<template>
  <div class="container">
    <div class="header">
      <h1>Color Pallete Generator</h1>
    </div>
    <div class="colors-grid">
      <div
        class="color-box"
        :style="{ background: color }"
        v-for="(color, idx) in colors"
        :key="idx"
      >
        {{ color }}
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.header {
  width: 100%;
  height: 10vh;
  background: rgb(62, 62, 62);
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  display: flex;
  flex-direction: column;
}

.colors-grid {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100vh;
  justify-content: space-between;
}

.color-box {
  width: 25%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: bold;
}
</style>
