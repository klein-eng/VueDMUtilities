<template>
  <h1>Initiative Tracker</h1>
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
  margin-top: 30px;
}
body {
  background-color: $background;
}
</style>
