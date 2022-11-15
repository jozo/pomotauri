<template>
<div>
    <div id="timer">{{ display }}</div>
    <div>
        <button v-if="intervalId" @click="stopTimer" class="btn-timer">
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
class Step {
    constructor(name, duration) {
        this.name = name
        this.duration = duration
    }
}


class Schedule {
    constructor() {
        this.steps = [
            new Step("work", 25*60),
            new Step("break", 5*60),
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
    data() {
        const schedule = new Schedule()
        return {
            intervalId: undefined,
            startDate: undefined,
            schedule: schedule,
            display: this.formatTimeSpan(schedule.currentStep().duration),
        }
    },
    methods: {
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
        },
        goToNextStep: function () {
            this.schedule.moveToNextStep()
            this.stopTimer()
        },
        notify: function () {
            let audio = document.getElementById('beepAudio');
            audio.play()
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
            return (number).toLocaleString('en-US', {minimumIntegerDigits: 2, useGrouping:false})
        },
        setTimerDisplay: function (text) {
            this.display = text
            document.title = text
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