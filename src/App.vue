<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Timer from './components/Timer.vue';
import Preferences from "./components/Preferences.vue";
</script>

<template>
  <div class="wrapper">
    <!-- FROM: https://www.freepik.com/free-vector/flat-design-zodiac-sign-collection_15186144.htm -->
    <img id="logo" alt="Pomotauri logo" src="./assets/logo.png" width="125"/>
    <Timer v-if="!displayPreferences"
           v-model:play-sound="preferences.playSound"
           v-model:display-preferences="displayPreferences"
           v-model:duration-work="preferences.durations.work"
           v-model:duration-break="preferences.durations.break"
           v-model:duration-long-break="preferences.durations.longBreak"
    />
    <Preferences
        v-model:play-sound="preferences.playSound"
        v-model:duration-work="preferences.durations.work"
        v-model:duration-break="preferences.durations.break"
        v-model:duration-long-break="preferences.durations.longBreak"
        @save="displayPreferences = !displayPreferences"
        v-else/>
    <audio id="beepAudio">
      <!-- Provide a link of beep sound from internet -->
      <source src="https://soundbible.com/mp3/Temple%20Bell-SoundBible.com-756181215.mp3">
    </audio>
  </div>
</template>

<script>
export default {
  data() {
    return {
      displayPreferences: false,
      preferences: {
        playSound: true,
        durations: {
          work: 25,
          break: 5,
          longBreak: 15,
        }
      }
    }
  },
}
</script>

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

.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#logo {
  margin: 25px 0;
}

button {
  padding: 8px 15px;
  background-color: #89c3ef;
  border: none;
  font-size: 1.2rem;
  color: #f0ffff;
}

button:hover {
  background-color: #004c99;
  cursor: pointer;
}

.buttons button:nth-of-type(1) {
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.buttons button:last-child {
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}
</style>
