
<template>
  <div>
    <grid-layout :layout.sync="layout" :col-num="3" :row-height="30" :is-draggable="true" :is-resizable="true" :is-mirrored="false" :vertical-compact="true" :margin="[10, 10]" :use-css-transforms="true">
      <grid-item v-for="(item, index) in layout" :value="item.id" class="flex flex-col border border-zinc-500 bg-zinc-700/20 rounded-lg text-center p-2 overflow-auto" :x="item.x" :y="item.y" :w="item.w" :h="item.h" :i="item.i" :key="item.i" @resize="resizeEvent" @resized="resizedEvent">
        <h2 class="text-2xl mt-2 mb-4">{{ item.i }}</h2>
        <span @click="deleteCard" class="material-symbols-rounded absolute right-4 top-4 cursor-pointer transition-all ease-linear duration-200 hover:bg-zinc-900/60 rounded-full p-1"> close </span>
        <component class="cursor-default" :user="user" :is="activeCards[index]" />
      </grid-item>
    </grid-layout>
    <div ref="eventsDiv" class="eventsJSON">
      <div v-for="event in eventLog"> {{ event }} </div>
    </div>
  </div>
</template>

<script>
import { ref, watch, defineComponent } from 'vue'
import { GridLayout, GridItem } from "vue-grid-layout"
import UsersList from './UsersList.vue'
import Credits from './Credits.vue'
import SocialMedia from './SocialMedia.vue'
import Websites from './Websites.vue'

export default defineComponent({
  name: 'Grid',
  props: {
    user: {
      type: String,
      required: true
    },
    card: {
      type: String | null,
      required: true,
    },
  },
  components: {
    GridLayout,
    GridItem,
    UsersList,
    Credits,
    SocialMedia,
    Websites
  },
  async beforeMount() {
    this.getComponentsFromLocalStorage()
    let lastCard
    let availableSpace
    this.activeCards?.forEach(card => {
      const cardIndex = this.cardSizes.findIndex((_card) => card.includes(_card.type))
      if (this.layout.length >= 1)
        lastCard = this.layout[this.layout.length - 1]

      lastCard?.w ? availableSpace = 3 - lastCard.w : 3

      if (this.layout.length >= 1 && lastCard.w < 3 && availableSpace >= this.cardSizes[cardIndex].layout.w)
        this.cardSizes[cardIndex].layout.x = lastCard.w

      if (cardIndex >= 0)
        this.layout.push(this.cardSizes[cardIndex].layout)
    })
  },

  methods: {
    getComponentsFromLocalStorage: function () {
      const getLocalComponents = JSON.parse(localStorage.getItem('currentComponents'));
      getLocalComponents.forEach(user => {
        if (user.id === +this.$props.user) {
          return this.activeCards = user.components
        }
      })
    },
    resizeEvent: function (i, newH, newW, newHPx, newWPx) {
      const msg = "RESIZE i=" + i + ", H=" + newH + ", W=" + newW + ", H(px)=" + newHPx + ", W(px)=" + newWPx
      this.eventLog.push(msg)
    },
    resizedEvent: function (i, newX, newY, newHPx, newWPx) {
      const msg = "RESIZED i=" + i + ", X=" + newX + ", Y=" + newY + ", H(px)=" + newHPx + ", W(px)=" + newWPx
      this.eventLog.push(msg)
    },

    deleteCard(event) {
      const card = event.srcElement.parentElement.getAttribute('value')
      const cardIndex = this.activeCards.findIndex(_card => {
        return _card === card
      })
      const layoutIndex = this.layout.findIndex(layout => {
        return layout.id === card
      })
      this.activeCards.splice(cardIndex, 1);
      this.layout.splice(layoutIndex, 1);

      const getLocalComponents = JSON.parse(localStorage.getItem('currentComponents'));
      const userLocalComponents = getLocalComponents.find(user => user.id === +this.user)

      userLocalComponents.components = this.activeCards

      localStorage.setItem('currentComponents', JSON.stringify(getLocalComponents))

    },

  },
  watch: {
    eventLog: function () {
      const eventsDiv = this.$refs.eventsDiv
      eventsDiv.scrollTop = eventsDiv.scrollHeight
    },
    card: function (component, oldComponent) {
      let lastCard
      let availableSpace

      const isActive = this.activeCards?.some((_component) => _component === component)

      const addCard = () => {

        this.activeCards.push(component)
        const cardIndex = this.cardSizes.findIndex((_card) => _card.type === component)

        if (this.layout.length >= 1)
          lastCard = this.layout[this.layout.length - 1]

        lastCard?.w ? availableSpace = 3 - lastCard.w : 3

        if (this.layout.length >= 1 && lastCard.w < 3 && availableSpace >= this.cardSizes[cardIndex].layout.w)
          this.cardSizes[cardIndex].layout.x = lastCard.w

        this.layout.push(this.cardSizes[cardIndex].layout)

        const getLocalComponents = JSON.parse(localStorage.getItem('currentComponents'));
        const userLocalComponents = getLocalComponents.find(user => user.id === +this.user)

        userLocalComponents.components.push(component)

        localStorage.setItem('currentComponents', JSON.stringify(getLocalComponents))
      }

      if (!isActive)
        addCard()
    },
  },
  data() {
    return {
      cardSizes: [
        {
          type: 'usersList', layout: { 'x': 0, 'y': 0, 'w': 2, 'h': 10, 'i': 'Users List', id: 'usersList' }
        },
        {
          type: 'credits', layout: { 'x': 0, 'y': 0, 'w': 1, 'h': 10, 'i': 'Credits', id: 'credits' }
        },
        {
          type: 'socialMedia', layout: { 'x': 0, 'y': 0, 'w': 3, 'h': 10, 'i': 'Scheduled Posts', id: 'socialMedia', }
        },
        {
          type: 'websites', layout: { 'x': 0, 'y': 0, 'w': 3, 'h': 10, 'i': 'Websites', id: 'websites', }
        }
      ],
      layout: [],
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