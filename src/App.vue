<template>
  <div class="container">
    <DiscgolfRound />
  </div>
</template>

<script>
import DiscgolfRound from './components/DiscgolfRound.vue'

export default {
  name: 'App',
  components: {
    DiscgolfRound
  }
}
</script>

<style>
:root {
  --col-white: hsl(160, 100%, 99%);
  --col-gray: hsl(0, 0%, 80%);
  --col-gray-light: hsl(0, 0%, 94%);
  --col-primary: hsla(157, 77%, 24%, 1);
}
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 5vmin;
  text-align: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.container{
  max-width: 1024px;
}
</style>
