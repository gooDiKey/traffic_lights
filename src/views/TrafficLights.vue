<template>
  <lights />
</template>

<script>
import Lights from '@/components/Lights.vue';

export default {
  components: {
    Lights,
  },
  mounted() {
    const requiredColors = ['red', 'yellow', 'green'];
      if (!requiredColors.find(el => el === this.$router.currentRoute.value.params.color)) {
        this.$router.push('/');
      }
  }
}
</script>

