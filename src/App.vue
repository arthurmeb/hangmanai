<template>
  <HomePage/>
</template>

<script>
import HomePage from './components/HomePage.vue'

export default {
  name: 'App',
  components: {
    HomePage
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  margin-top: 0;
}
@font-face {
  font-family: 'space-grotesk';
  src: url('../public/fonts/SpaceGrotesk-Bold.ttf') format('truetype');
  font-weight: bold;
}
@font-face {
  font-family: 'space-grotesk';
  src: url('../public/fonts/SpaceGrotesk-Light.ttf') format('truetype');
  font-weight: light;
}
@font-face {
  font-family: 'space-grotesk';
  src: url('../public/fonts/SpaceGrotesk-Medium.ttf') format('truetype');
  font-weight: medium;
}
@font-face {
  font-family: 'space-grotesk';
  src: url('../public/fonts/SpaceGrotesk-Regular.ttf') format('truetype');
  font-weight: regular;
}
@font-face {
  font-family: 'space-grotesk';
  src: url('../public/fonts/SpaceGrotesk-SemiBold.ttf') format('truetype');
  font-weight: semibold;
}

</style>
