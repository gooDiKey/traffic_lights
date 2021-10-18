<template>
  <div class="lights">

    <div class="lights-bg">
      <div class="top-circle"></div>
      <div class="middle-area"></div>
      <div class="bottom-circle"></div>
    </div>

    <div class="lights-wrapper">
      <light
        v-for="light in lights"
        :light="light"
        :key="light.name"
        :timer="timer"
        :class="{ active: light.isActive }"
        @lightTime:end="changeLight"
      />
    </div>

  </div>
</template>

<script>
import Light from '@/components/Light';

export default {
  components: {
    Light,
  },
  data() {
    return {
      lights: [
        {name: 'red', color: '#b53c14', totalTime: 10, isActive: false},
        {name: 'yellow', color: '#fce300', totalTime: 3, isActive: false},
        {name: 'green', color: '#3ab514', totalTime: 15, isActive: false},
      ],
      currentColor: 'red',
      lastColor: 'yellow',
      timer: 10,
    }
  },
  methods: {
    changeLight() {
      let color = this.getNextLightColor();
      
      this.lastColor = localStorage.lastColor = this.currentColor;
      this.currentColor = localStorage.currentColor = color;

      this.lights.filter(light => {
        if (light.name === this.currentColor || light.name === this.lastColor) {
          if (light.name === this.currentColor) {
            this.timer = light.totalTime;
          }
          
          light.isActive = !light.isActive;
        }
      });

      this.$router.push(`/${this.currentColor}`);
    },
    getNextLightColor() {
      let color;

      switch (this.currentColor) {
        case 'red':
          color = 'yellow';
          break;
        case 'yellow':
          if (this.lastColor === 'green') {
            color = 'red';
          } else {
            color = 'green';
          }
          break;
        case 'green':
          color = 'yellow';
          break;
      }

      return color
    }
  },
  mounted() {
    if (
      localStorage.currentColor &&
      localStorage.lastColor &&
      localStorage.timer &&
      (localStorage.currentColor === this.$router.currentRoute.value.params.color)
    ) {

      this.currentColor = localStorage.currentColor; 
      this.lastColor = localStorage.lastColor;
      this.timer = localStorage.timer;
    } else {
    this.currentColor = localStorage.currentColor = this.$router.currentRoute.value.params.color;
    
    if (this.currentRoute === 'green' || this.currentRoute === 'red') {
        this.lastColor = localStorage.lastColor = 'yellow';
    } else if (this.currentRoute === 'yellow') {
      this.lastColor = localStorage.lastColor = ['green', 'red'][Math.random()];
    }

    localStorage.timer = this.timer = this.lights.find(light => light.name === this.currentColor ).totalTime;
    }

    this.lights.forEach(light => {
      if (light.name === this.$router.currentRoute.value.params.color) {
        light.isActive = true;
      }
    });
  },
}
</script>

<style scroped>
.lights {
  position: relative;
  width: 300px;
  height: 800px;
}

.lights-bg {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 1;
}

.lights-wrapper {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  padding: 40px 0;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 2;
}

.top-circle, .bottom-circle {
  position: absolute;
  width: 300px;
  height: 300px;
  background-color: black;
  border-radius: 50%;
}

.top-circle {
  top: 0px;
  left: 0px;
}

.bottom-circle {
  top: 500px;
  left: 0px;
}

.middle-area {
  position: absolute;
  width: 300px;
  height: 500px;
  top: 150px;
  left: 0px;
  background-color: black;
}
</style>