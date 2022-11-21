<template>
<div>
    <div id="timer">{{ display }}</div>
    <div>
        <button v-if="isRunning" @click="stopTimer" class="btn-timer">
            Stop timer
        </button>
        <button v-else @click="startTimer" class="btn-timer">
            Start timer
        </button>
        <button @click="goToNextStep" class="btn-timer" title="Next step">
            >
        </button>
    </div>
</div>
</template>

<script>
const STEPS = [
    {name: "work", duration: 25*60},
    {name: "break", duration: 5*60},
    {name: "work", duration: 25*60},
    {name: "break", duration: 5*60},
    {name: "long break", duration: 15*60},
]

function doubleDigit(number) {
    return (number).toLocaleString('en-US', {minimumIntegerDigits: 2, useGrouping:false})
}

function formatTimeSpan(spanSeconds) {
    const minutes = Math.floor(spanSeconds / 60)
    const seconds = Math.floor(spanSeconds - minutes * 60)
    return doubleDigit(minutes) + ":" + doubleDigit(seconds)
}

function notify() {
    let audio = document.getElementById('beepAudio');
    audio.play()
}

export default {
    data() {
        return {
            stepIndex: 0,
            intervalId: undefined,
            startDate: undefined,
            currentDate: undefined,
        }
    },
    computed: {
        isRunning() {
            return Boolean(this.intervalId)
        },
        currentStep() {
            const idx = this.stepIndex % STEPS.length
            return STEPS[idx]
        },
        secondsLeftInStep() {
            const diffSeconds = (this.currentDate - this.startDate) / 1000
            return this.currentStep.duration - diffSeconds
        },
        display() {
            const seconds = this.isRunning ? this.secondsLeftInStep : this.currentStep.duration
            const text = formatTimeSpan(seconds)
            document.title = text + " Pomotauri"
            return text
        }
    },
    methods: {
        startTimer: function () {
            this.startDate = new Date()
            this.currentDate = this.startDate

            this.intervalId = setInterval(() => {
                this.currentDate = new Date()
                if (this.secondsLeftInStep <= 0) {
                    this.goToNextStep()
                    notify()
                }
            }, 250)
        },
        stopTimer: function () {
            clearInterval(this.intervalId)
            this.intervalId = null
        },
        goToNextStep: function () {
            this.stopTimer()
            this.stepIndex++
        }
    }
}
</script>

<style>
#timer {
    font-size: 2.5rem;
    font-weight: bold;
    margin-bottom: 25px;
}

.btn-timer {
    margin: 0 5px;
    padding: 8px 15px;
    background-color: #89c3ef;
    border: none;
    border-radius: 5px;
    font-size: 1.2rem;
    color: #f0ffff;
}

.btn-timer:hover {
    background-color: #004c99;
    cursor: pointer;
}
</style>