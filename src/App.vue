<template>
  <div id="app">
    <Smartpull/>
  </div>
</template>

<script>
  import Smartpull from './components/smartpull.vue'

export default {
  name: 'app',
  components: {
      Smartpull
      
  },
    data() {
      return {
          todos : []
      }
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
  margin-top: 40px;
}
</style>