<script>
import { ref, watch, defineComponent } from 'vue'
export default defineComponent({
  props: {
  },
  async created() {
    const response = await fetch("https://run.mocky.io/v3/37b71c8a-54ca-4050-8e82-11f9078b7786");
    const data = await response.json();
    console.log(data)
    this.users = data.data;
  },
  data() {
    return {
      users: {},
    }
  },
  methods: {
    onAddToGrid: function (event) {
      const value = event.target.getAttribute('value')
      this.$emit('handle-add-to-grid', value)
    },
    hoverHeader: function () {
      this.$emit('handle-hover-header', true)
    },
    unHoverHeader: function () {
      this.$emit('unhandle-hover-header', false)
    },
  }
})
</script>

<template>
  <div @mouseover="hoverHeader" @mouseleave="unHoverHeader" class="header bg-indigo-800 h-screen w-14 hover:w-64 absolute left-0 top-0 overflow-hidden flex flex-col   ease-linear duration-200">
    <div class="menu flex flex-col justify-center mt-2">
      <div class="menu mb-10">
        <span class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> menu </span>
      </div>
      <div @click="onAddToGrid" value="websites" class="flex flex-row items-center cursor-pointer self-start">
        <p value="websites" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> public </p>
        <p value="websites">Websites</p>
      </div>
      <div @click="onAddToGrid" value="scheduledPosts" class="flex flex-row items-center cursor-pointer  self-start">
        <span value="scheduledPosts" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> tag </span>
        <p value="scheduledPosts" class="left-14 whitespace-nowrap">Social media</p>
      </div>
      <div @click="onAddToGrid" value="credits" class="flex flex-row items-center cursor-pointer  self-start">
        <span value="credits" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> local_atm </span>
        <p value="credits" class="left-14 whitespace-nowrap">Credits</p>
      </div>
      <div @click="onAddToGrid" value="usersList" class="flex flex-row items-center cursor-pointer  self-start">
        <span value="usersList" class="material-symbols-rounded cursor-pointer text-4xl w-14 flex justify-center"> account_circle </span>
        <p value="usersList" class="left-14 whitespace-nowrap">Users</p>
      </div>
    </div>
  </div>
</template>


<style>

</style>