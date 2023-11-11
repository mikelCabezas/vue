
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
    <grid-item v-for="(item, index) in layout" class="flex flex-col border border-zinc-500 bg-zinc-700/20 rounded-lg text-center p-2 overflow-auto" 
      :x="item.x" 
      :y="item.y" 
      :w="item.w" 
      :h="item.h" 
      :i="item.i" 
      :key="item.i" 
      @resize="resizeEvent"
      @resized="resizedEvent"
    >
    <h2 class="text-2xl mt-2 mb-4">{{ item.i }}</h2>
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
<script>
import { ref, watch, defineComponent } from 'vue'
import { GridLayout, GridItem } from "vue-grid-layout"
import UsersList from './UsersList.vue'
import Credits from './Credits.vue'
import ScheduledPosts from './ScheduledPosts.vue'

export default defineComponent({
  name: 'Grid',
  props: {
    msg: {
      type: String,
      required: true,
    },
    card: {
      type: String | null,
      required: true,
    },
  },
  components: {
    GridLayout,
    GridItem,
    UsersList,
    Credits,
    ScheduledPosts
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
      addToGrid: function(event) {
        console.log(event.target.value)
        const component = event.target.value
        const componentLayout = componentLayouts.i.find(_component => _component === component)
      }
  },
  watch: {
    eventLog: function() {
      const eventsDiv = this.$refs.eventsDiv;
      eventsDiv.scrollTop = eventsDiv.scrollHeight;
    },
    card: function(component, oldComponent) {
      let lastCard
      let availableSpace

      console.log(component, oldComponent)
      
      const isActive = this.activeCards.some((_component) => _component === component)

      const addCard = () => {
        this.activeCards.push(component)
        
        const cardIndex = this.cardSizes.findIndex((_card) => _card.type === component)
  
        if (this.layout.length >= 1) 
          lastCard = this.layout[this.layout.length - 1]
  
        lastCard?.w ? availableSpace = 3 - lastCard.w : 3
  
        if(this.layout.length >= 1 && lastCard.w < 3 && availableSpace >= this.cardSizes[cardIndex].layout.w) 
          this.cardSizes[cardIndex].layout.x = lastCard.w
        
        this.layout.push(this.cardSizes[cardIndex].layout)
      }

      if(!isActive)
        addCard()
    }
  },
  data() {
    return {
      cardSizes: [
        {type: 'usersList', layout: {'x': 0, 'y': 0, 'w': 2, 'h': 10,'i': 'Users List'}},
        {type: 'credits', layout: {'x': 0, 'y': 0, 'w': 1, 'h': 10,'i': 'Credits'}},
        {type: 'scheduledPosts', layout: {'x': 0, 'y': 0, 'w': 3, 'h': 10,'i': 'Scheduled Posts'}}
      ],
      layout: [
        // { "x": 0, "y": 0, "w": 3, "h": 7, "i": "Component 1" },
        // { "x": 0, "y": 7, "w": 1, "h": 4, "i": "Component 2" },
        // { "x": 1, "y": 7, "w": 1, "h": 5, "i": "Component 3" },
        // { "x": 2, "y": 7, "w": 1, "h": 5, "i": "Component 4" },
      ],
      activeCards: [],
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
