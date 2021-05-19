<template>
  <Dolist />
</template>

<script>
import Dolist from './components/Dolist.vue'

export default {
  name: 'App',
  components: {
    Dolist
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
