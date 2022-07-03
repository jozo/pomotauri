<template>
<div>
    <div id="timer">
        {{ timerMinutes}}m:{{ timerSeconds }}s
    </div>
    <div>
        <button v-if="isRunning" @click="stopTimer" class="btn-timer">
            Stop timer
        </button>
        <button v-else @click="startTimer" class="btn-timer">
            Start timer
        </button>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            isRunning: false,
            intervalId: undefined,
            startDate: undefined,
            timerMinutes: 25,
            timerSeconds: 0,
            duration: 25*60   // seconds
        }
    },
    methods: {
        startTimer: function () {
            this.startDate = new Date()
            this.isRunning = true

            this.intervalId = setInterval(() => {
                let now = new Date()
                let diffSeconds = (now - this.startDate) / 1000
                let leftSeconds = this.duration - diffSeconds

                this.timerMinutes = Math.floor(leftSeconds / 60)
                this.timerSeconds = Math.floor(leftSeconds - this.timerMinutes * 60)

                if (leftSeconds <= 0) {
                    this.stopTimer()
                    this.notify()
                }
            }, 100)
        },
        stopTimer: function () {
            clearInterval(this.intervalId)
            this.isRunning = false
            this.timerMinutes = 25
            this.timerSeconds = 0
        },
        notify: function () {
            let audio = document.getElementById('beepAudio');
            audio.play()
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