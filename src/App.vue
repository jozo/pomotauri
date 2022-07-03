<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
import Timer from './components/Timer.vue';
</script>

<template>
  <!-- FROM: https://www.freepik.com/free-vector/flat-design-zodiac-sign-collection_15186144.htm -->
  <img id="logo" alt="Pomotauri logo" src="./assets/logo.png" width="150"/>
  <Timer />
  <audio id="beepAudio" >
    <!-- Provide a link of beep sound from internet -->
    <source src= "https://www.soundjay.com/buttons/beep-06.wav">
  </audio>
</template>

<style>
body {
  background-color: #f0ffff;
}

#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#logo {
  margin: 25px 0;
}
</style>
