<template>
  <div
    class="light"
    :class="{ blink: ending }"
    :style="{'--color': light.color}"
  >
    <timer 
      v-if="light.isActive" 
      :name="light.name" 
      :timer="timer"
      @timer:end="lightEnd"
      @timer:soonEnd="endingChange"
    />
  </div>
</template>

<script>
import Timer from '@/components/Timer.vue';

export default {
  name: 'Light',
  components: {
    Timer,
  },
  data() {
    return {
      ending: false,
    }
  },
  props: {
    light: {
      type: Object,
      required: true,
    },
    timer: {
      type: Number,
      required: true,
    }
  },
  methods: {
    endingChange() {
      this.ending = true;
    },
    lightEnd() {
      this.ending = false;
      this.$emit('lightTime:end')
    },
  }
}
</script>

<style scoped>
.light {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  border-radius: 50%;
  z-index: 2;
  background-color: var(--color);
  filter: brightness(0.3);
}

.active {
  box-shadow: 0 0 60px var(--color);
  filter: brightness(1);
}

.blink {
    animation: blinking 1s ease-in-out;
    animation-iteration-count: infinite;
    animation-direction: alternate;
}

@keyframes blinking {
    from {
        filter: brightness(1);
    }
    to {
        filter: brightness(0.3);
    }
}
</style>