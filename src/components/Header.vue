<template>
  <div @mouseover="hoverHeader" @mouseleave="unHoverHeader" class="header bg-indigo-800 h-screen w-14 hover:w-64 fixed left-0 top-0 overflow-hidden flex flex-col ease-linear duration-200" :class="{ headerStatus }">
    <div class="menu flex flex-col justify-center mt-2 h-full pb-4">
      <div class="menu mb-10">
        <span class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> menu </span>
      </div>
      <div @click="onAddToGrid" v-for="service of services" class="flex flex-row items-center cursor-pointer self-start w-max">
        <p v-if="service.status" :value="service.value" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> {{ service.icon }} </p>
        <p v-if="service.status" :value="service.value">{{ service.service }}</p>
      </div>
      <div @click="onAddToGrid" value="credits" class="flex flex-row items-center cursor-pointer  self-start">
        <span value="credits" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> local_atm </span>
        <p value="credits" class="left-14 whitespace-nowrap">Credits</p>
      </div>
      <div @click="onAddToGrid" value="usersList" class="flex flex-row items-center cursor-pointer  self-start mb-auto">
        <span value="usersList" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> account_circle </span>
        <p value="usersList" class="left-14 whitespace-nowrap">Users</p>
      </div>
      <div class="users  relative bottom-40z">
        <div @click="toggleUser" v-if="currentUser" id="dropdownDefaultButton" data-dropdown-toggle="dropdown" class="mt-auto text-white font-medium text-lg  text-center flex items-center cursor-pointer w-max" type="button">
          <img class="cursor- w-14 flex justify-center rounded-full mr-2 p-2" :src="currentUser.avatar" :alt="currentUser.name"> {{ currentUser.name }} <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4" />
          </svg>
        </div>
        <div value="user" class="flex flex-row items-center cursor-pointer w-full self-start " :class="{ hidden: !toggleDropdown }">
          <div id="dropdown" class="z-10 bg-white divide-y divide-gray-100 rounded-lg shadow w-full dark:bg-gray-700">
            <ul v-if="currentUser" class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownDefaultButton">
              <li v-for="user of users">
                <div v-if="user.id !== currentUser.id">
                  <router-link :to="{
                    name: 'dashboard',
                    params: { userId: user.id },
                    force: true
                  }" @click.native="refreshPage">
                    <div value="usersList" class="flex flex-row items-center cursor-pointer self-start text-zinc-100 font-bold text-lg">
                      <img class="cursor- w-14 flex justify-center rounded-full mr-2 p-2" :src="user.avatar" :alt="user.name"> {{ user.name }}
                    </div>
                  </router-link>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    users: {
      type: Array,
      required: true
    },
    userId: {
      type: String,
      required: true
    },
  },

  watch: {
    users: async function () {
      const currentUser = await this.users.find((user) => {
        return user.id === +this.userId
      })
      if (currentUser) {
        this.services.forEach(service => {
          if (currentUser.products.includes(service.value))
            service.status = true
        })
      }
      this.getCurrentUser()
    }
  },
  data() {
    return {
      services: [
        { service: 'Websites', value: 'websites', icon: 'public', status: false },
        { service: 'Social Media', value: 'socialMedia', icon: 'tag', status: false }
      ],
      headerStatus: false,
      toggleDropdown: false,
      currentUser: null
    }
  },
  methods: {
    onAddToGrid: function (event) {
      const value = event.target.getAttribute('value')
      this.$emit('handle-add-to-grid', value)
    },
    hoverHeader: function () {
      this.$emit('handle-hover-header', true)
      this.headerStatus = true
    },
    unHoverHeader: function () {
      this.$emit('unhandle-hover-header', false)
      this.headerStatus = false
      this.toggleDropdown = false
    },
    toggleUser: function () {
      this.toggleDropdown = !this.toggleDropdown
    },
    getCurrentUser: function () {
      this.currentUser = this.users.find((user) => user.id === +this.userId)
    },
    refreshPage() {
      location.reload();
    }
  },
})
</script>

