<script setup>
import { onMounted, ref } from 'vue';

import { createToaster } from '@meforma/vue-toaster';

const toaster = createToaster({
  maxToasts: 3,
  duration: 2000
});

const colors = ref(['', '', '', '', '']);

const generateRandomHex = () => {
  colors.value = colors.value.map((key) => {
    return (
      '#' + Math.floor(Math.random() * 16777215).toString(16)
    ).toUpperCase();
  });
};

const copyClipBoard = async (color) => {
  try {
    await navigator.clipboard.writeText(color);
    toaster.show('Copied to clipboard!', {
      position: 'top-right',
    });
  } catch (err) {
    toaster.show('Something went wrong!', {
      position: 'top-right',
    });
  }
};

const getContrast = (hexcolor) => {
  if (hexcolor.slice(0, 1) === '#') {
    hexcolor = hexcolor.slice(1);
  }

  const r = parseInt(hexcolor.substr(0, 2), 16);
  const g = parseInt(hexcolor.substr(2, 2), 16);
  const b = parseInt(hexcolor.substr(4, 2), 16);

  const yiq = (r * 299 + g * 587 + b * 114) / 1000;
  return yiq >= 128 ? 'black' : 'white';
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
  <div class="container" @keypress="generateRandomHex()">
    <div class="header">
      <h1>Color Pallete Generator</h1>
    </div>
    <div class="colors-grid">
      <div
        class="color-box"
        :style="{ background: colorHex }"
        v-for="(colorHex, idx) in colors"
        :key="idx"
        @click="copyClipBoard(colorHex)"
      >
        <span :style="{ color: getContrast(colorHex) }">{{ colorHex }}</span>
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
  font-size: 1.8rem;
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
  cursor: default;
  font-size: 1.8rem;
  user-select: none;
  color: white;
}

.color-box:hover:before {
  animation: animation 0.15s ease;
  position: absolute;
  top: 55%;
  content: url(https://api.iconify.design/iconoir:clipboard-check.svg?color=%23888888&height=30);
}

@keyframes animation {
  from {
    transform: scale(0);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.do-contrast {
  color: color-contrast(black vs white, black);
  width: 100px;
  height: 100px;
  background: black;
}
</style>
