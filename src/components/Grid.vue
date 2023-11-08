<script>
import { ref, watch, defineComponent } from 'vue'
import { GridLayout, GridItem } from "vue-grid-layout"
import UsersList from './UsersList.vue'

export default defineComponent({
  name: 'Grid',
  props: {
    msg: {
      type: String,
      required: true,
    },
  },
  components: {
    GridLayout,
    GridItem,
    UsersList
  },
  methods: {
     resizeEvent: function(i, newH, newW, newHPx, newWPx){
          const msg = "RESIZE i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx;
          this.eventLog.push(msg);
          console.log(msg);
      },
      resizedEvent: function(i, newX, newY, newHPx, newWPx){
            const msg = "RESIZED i=" + i + ", X=" + newX + ", Y=" + newY + ", H(px)=" + newHPx + ", W(px)=" + newWPx;
            this.eventLog.push(msg);
            console.log(msg);
      },
  },
  watch: {
    eventLog: function() {
      const eventsDiv = this.$refs.eventsDiv;
      eventsDiv.scrollTop = eventsDiv.scrollHeight;
    }
  },
  data() {
    return {
      layout: [
        { "x": 0, "y": 0, "w": 3, "h": 7, "i": "hola" },
        { "x": 0, "y": 7, "w": 1, "h": 4, "i": "mi" },
        { "x": 1, "y": 7, "w": 1, "h": 5, "i": "kel" },
        { "x": 2, "y": 7, "w": 1, "h": 5, "i": "C" },
      ],
      activeCards: [UsersList],
      components: [],
      eventLog: []

    }
  },
  setup() {
    const count = ref(0)
    return { count }
  },
  
})
</script>

<template>
<div>

  <grid-layout class="" 
  :layout.sync="layout" 
  :col-num="3" 
  :row-height="30" 
  :is-draggable="true" 
  :is-resizable="true" 
  :is-mirrored="false" 
  :vertical-compact="true" 
  :margin="[10, 10]" 
  :use-css-transforms="true">
    <grid-item v-for="(item, index) in layout" class="border border-zinc-500 bg-zinc-700/20 rounded-lg text-center p-2 overflow-auto" 
      :x="item.x" 
      :y="item.y" 
      :w="item.w" 
      :h="item.h" 
      :i="item.i" 
      :key="item.i" 
      @resize="resizeEvent"
      @resized="resizedEvent"
    >
    <h2 class="text-2xl">{{ item.i }}</h2>
        <component :is="activeCards[index]" />
     </grid-item>
  </grid-layout>
  <div ref="eventsDiv" class="eventsJSON">
    <div v-for="event in eventLog">
        {{event}}
    </div>
  </div>
  </div>
</template>

<style scoped>
a {
  color: #42b983;
}

label {
  margin: 0 0.5em;
  font-weight: bold;
}

code {
  background-color: #eee;
  padding: 2px 4px;
  border-radius: 4px;
  color: #304455;
}
</style>
