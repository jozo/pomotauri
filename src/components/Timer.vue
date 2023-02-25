<template>
  <div id="timer">{{ display }}</div>
  <div class="buttons">
    <button @click="toggleTimer" class="btn-timer" title="Start/Stop">
      <span v-if="intervalId">⏹</span>
      <span v-else>⏵</span>
    </button>
    <button @click="goToNextStep" class="btn-timer" title="Next step">⏭</button>
    <button class="btn-timer" title="Preferences"
            @click="togglePreferences">
      ⛭
    </button>
  </div>
</template>

<script>
class Step {
  constructor(name, duration) {
    this.name = name
    this.duration = duration
  }
}

class Schedule {
  constructor(workMins, breakMins, longBreakMins) {
    const workStep = new Step("work", workMins * 60)
    const breakStep = new Step("break", breakMins * 60)

    this.steps = [
      workStep, breakStep, workStep, breakStep, workStep,
      new Step("break", longBreakMins * 60),
    ]
    this.stepIndex = 0
  }

  currentStep() {
    const idx = this.stepIndex % this.steps.length
    return this.steps[idx]
  }

  moveToNextStep() {
    this.stepIndex++
  }
}

export default {
  props: {
    durationWork: Number,
    durationBreak: Number,
    durationLongBreak: Number,
    displayPreferences: Boolean,
    playSound: Boolean,
  },
  data() {
    const schedule = new Schedule(this.durationWork, this.durationBreak, this.durationLongBreak)
    return {
      intervalId: undefined,
      startDate: undefined,
      schedule: schedule,
      display: this.formatTimeSpan(schedule.currentStep().duration),
    }
  },
  methods: {
    toggleTimer: function () {
      if (this.intervalId) {
        this.stopTimer()
      } else {
        this.startTimer()
      }
    },
    startTimer: function () {
      this.startDate = new Date()

      this.intervalId = setInterval(() => {
        const leftSeconds = this.secondsLeftInStep()
        this.setTimerDisplay(this.formatTimeSpan(leftSeconds))

        if (leftSeconds <= 0) {
          this.schedule.moveToNextStep()
          this.stopTimer()
          this.notify()
        }
      }, 100)
    },
    stopTimer: function () {
      clearInterval(this.intervalId)
      this.intervalId = null
      this.setTimerDisplay(this.formatTimeSpan(this.schedule.currentStep().duration))
      document.title = "Pomotauri"
    },
    goToNextStep: function () {
      this.schedule.moveToNextStep()
      this.stopTimer()
    },
    notify: function () {
      if (this.playSound) {
        let audio = document.getElementById('beepAudio');
        audio.play()
      }
    },
    secondsLeftInStep: function () {
      const diffSeconds = (new Date() - this.startDate) / 1000
      return this.schedule.currentStep().duration - diffSeconds
    },
    formatTimeSpan: function (spanSeconds) {
      const minutes = Math.floor(spanSeconds / 60)
      const seconds = Math.floor(spanSeconds - minutes * 60)
      return this.doubleDigit(minutes) + ":" + this.doubleDigit(seconds)
    },
    doubleDigit: function (number) {
      return (number).toLocaleString('en-US', {minimumIntegerDigits: 2, useGrouping: false})
    },
    setTimerDisplay: function (text) {
      this.display = text
      document.title = text
    },
    togglePreferences: function () {
      this.stopTimer()
      this.$emit('update:displayPreferences', !this.displayPreferences)
    }
  }
}
</script>

<style>
#timer {
  font-size: 3.5rem;
  font-weight: bold;
  margin-bottom: 25px;
  line-height: 1;
}


</style>