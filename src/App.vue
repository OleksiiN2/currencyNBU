<template>
  <div id="app">
    
    <currency/>
  </div>
</template>

<script>
import currency from './components/currency.vue'

export default {
  name: 'app',
  components: {
    currency
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
  margin-top: 50px;
  background: #017d47;
}
html{
   background: #017d47;
}
</style>
