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
  data() {
    return {
      component: null
    }
  },
  methods: {
     handleAddToGrid:  function(event) {
      console.log(event.target.value)
      this.component = event.target.value
    }
  },
  
})
</script>

<template>
  <div id="app">
    <Header />
    <button v-on:click="handleAddToGrid" value="usersList">Users List</button>
    <button v-on:click="handleAddToGrid" value="credits">Credits</button>
    <button v-on:click="handleAddToGrid" value="scheduledPosts">Scheduled posts</button>
    <Grid msg="Hello Vue 2 + Vite" :card="component" />
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
