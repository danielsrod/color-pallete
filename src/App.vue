<script setup>
import { defineComponent } from '@vue/runtime-core';
import ColorPallete from './components/ColorPallete.vue';

defineComponent({
  ColorPallete,
});
</script>

<template>
  <ColorPallete></ColorPallete>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
