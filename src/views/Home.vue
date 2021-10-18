<template>
</template>

<script>

export default {
  name: 'Home',
  mounted() {
    let redirect = 'red';

    if (localStorage.currentColor) {
        redirect = localStorage.currentColor;
    }

    this.$router.push(`/${redirect}`);
  }
}
</script>