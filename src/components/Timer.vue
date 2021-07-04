<template>
  <div id="timer">
    <section class="nes-container with-title">
      <h3 class="title">kintre-timer</h3>
      <div class="nes-field is-inline">
        <label for="work_field">Work</label>
        <input type="number" id="work_field" class="nes-input" v-model="work"/>
      </div>
      <div class="nes-field is-inline">
        <label for="rest_field">Rest</label>
        <input type="number" id="rest_field" class="nes-input" v-model="rest"/>
      </div>
      <div class="nes-field is-inline">
        <label for="set_field">Set</label>
        <input type="number" id="set_field" class="nes-input" v-model="set"/>
      </div>
      <div class="btn-area">
        <button class="nes-btn is-primary" v-on:click="start" v-bind:class="{ 'is-disabled': timerOn }">Start</button>
        <button class="nes-btn is-primary" v-on:click="reset" v-bind:class="{ 'is-disabled': !timerOn }">Reset</button>
      </div>
      <div class="btn-area">
        <button class="nes-btn is-success" v-on:click="setSec(30)">30s</button>
        <button class="nes-btn is-warning" v-on:click="setSec(60)">60s</button>
        <button class="nes-btn is-error" v-on:click="setSec(120)">120s</button>
      </div>
    </section>

  </div>

</template>

<script>
export default {
  name: 'timer',
  data() {
    return {
      work: 0,
      work_bk: 0,
      rest: 0,
      rest_bk: 0,
      set: 0,
      timerOn: false,
      timerObj: null,
      audio: new Audio(require('@/assets/Countdown01-1.mp3'))
    }
  },

  methods: {
    count: function() {
      if (this.work <= 0) {
        if (this.set <= 1) {
          this.reset()
        } else if (this.rest >= 1) {
          this.rest--
        } else if (this.rest <= 0) {
          this.work = this.work_bk
          this.rest = this.rest_bk
          this.set--
        }
      } else {
        this.work--
      }
      if (this.work === 2 || this.rest === 2) {
        this.audio.play()
      }
    },

    start: function() {
      const self = this
      this.work_bk = this.work
      this.rest_bk = this.rest
      this.audio.play()
      setTimeout(() => {
        this.timerObj = setInterval(function() { self.count() }, 1000)
      }, 2000)
      this.timerOn = true
    },

    reset: function() {
      clearInterval(this.timerObj)
      this.timerOn = false
      this.work = 0
      this.rest = 0
      this.set = 0
    },

    setSec: function(sec) {
      clearInterval(this.timerObj)
      this.timerOn = false
      this.work = sec
      this.rest = Math.ceil(sec / 4)
      this.set = 6
    }
  }

}
</script>

<style scoped>
.nes-field {
  margin-bottom: 15px;
}
.nes-btn {
  margin: 10px 6px;
}
</style>
