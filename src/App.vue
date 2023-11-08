<script>
import { defineComponent } from 'vue'
import Grid from './components/Grid.vue'
import Header from './components/Header.vue'

export default defineComponent({
  name: 'App',
  components: {
    Grid,
    Header
  },
})
</script>

<template>
  <div id="app">
    <Header />
    <Grid msg="Hello Vue 2 + Vite" />
  </div>
</template>
  <!-- 
  <style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  </style> -->
