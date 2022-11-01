<template>
  <div id="app">
    <img src="./assets/logo.png">
    <!-- <HelloWorld/>
    <Counter/> -->
    <HomeView/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld'
// import Counter from './components/Counter'
import HomeView from './components/HomeView'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    // Counter
    HomeView
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>