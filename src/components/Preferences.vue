<template>
  <div>
    <div class="row">
      <input type="checkbox" id="sounds" :checked="playSound"
             @click="$emit('update:playSound', $event.target.checked)"
      >
      <label for="sounds">Play notification sound</label>
    </div>
    <div class="row">
      <div>
        <input type="number" min="1" :value="durationWork"
               @input="$emit('update:durationWork', $event.target.value)">
      </div>
      <div>(mins) Work step</div>
    </div>
    <div class="row">
      <div>
        <input type="number" min="1" :value="durationBreak"
               @input="$emit('update:durationBreak', $event.target.value)">
      </div>
      <div>(mins) Break</div>
    </div>
    <div class="row">
      <div>
        <input type="number" min="1" :value="durationLongBreak"
               @input="$emit('update:durationLongBreak', $event.target.value)">
      </div>
      <div>(mins) Long break</div>
    </div>
  </div>
  <div class="buttons">
    <button @click="$emit('save')">Save</button>
  </div>
</template>

<script>
export default {
  name: "Preferences",
  props: {
    playSound: Boolean,
    durationWork: Number,
    durationBreak: Number,
    durationLongBreak: Number,
  },
  emits: ["update:playSound", "update:durationWork",
    "update:durationBreak", "update:durationLongBreak", "save"]
}
</script>

<style scoped>
.row {
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
}

input[type=number] {
  width: 50px;
  display: block;
  margin-right: 10px;
  font-weight: bold;
}
</style>