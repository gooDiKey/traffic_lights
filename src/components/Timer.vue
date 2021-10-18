<template>
  <div class="timer">
    {{ time }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      time: 0,
    }
  },
  props: {
    name: {
      type: String,
      required: true,
    },
    timer: {
      type: Number,
      required: true,
    }
  },
  methods: {
    async start() {
      let timer = setInterval(() => {
        --this.time
        localStorage.timer = this.time;
        
        if (this.time <= 0) {
            clearInterval(timer);
            this.$emit('timer:end')
        } else if (this.time === 3) {
          this.$emit('timer:soonEnd');
        }
      }, 1000);
    },
  },
  mounted() {
    this.time = this.timer;
    this.start();
  }
}
</script>

<style>
.timer {
    color:white;
    font-family: 'Orbitron';
    font-size: 100px;
    text-shadow: 0 0 10px lightgrey;
}
</style>