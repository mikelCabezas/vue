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
      component: null,
      headerMainSpacing: 'padding-left: 5rem;'
    }
  },
  methods: {
    handleAddToGrid: function (value) {
      this.component = value
    },
    handleHoverHeader: function (value) {
      this.headerMainSpacing = 'padding-left: 18rem;'
    },
    unHandleHoverHeader: function (value) {
      this.headerMainSpacing = 'padding-left: 5rem;'
    },
  },

})
</script>

<template>
  <!-- <div id="app" class="ease-linear duration-200" :style="headerMainSpacing">
      <Header @handle-add-to-grid="handleAddToGrid" @handle-hover-header="handleHoverHeader" @unhandle-hover-header="unHandleHoverHeader" />
      <Grid msg="Hello Vue 2 + Vite" :card="component" />
    </div> -->
  <router-view />
</template>

