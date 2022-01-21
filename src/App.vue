<template>
  <Root/>
</template>

<script>
import Root from './components/Root.vue'

export default {
  name: 'App',
  components: {
    Root
  }
}
</script>

<style lang="scss">
@import './sass/variables.scss';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: $white;
}
body {
  background-color: $background;
}
</style>
