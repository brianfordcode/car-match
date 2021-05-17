<template>
  <h1 class="title">Car Match</h1>
  <options></options>
</template>

<script>
import options from './components/options'

export default {
  name: 'App',
  components: {options}
}
</script>

<style>

* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.title {
  text-align: center;
  margin-top: 30px;
}
</style>
