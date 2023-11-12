
<template>
<div>
<div v-for="(website, index) of websites" class="flex flex-col justify-start w-full gap-4 p-4 bg-white rounded-xl dark:bg-zinc-700/60 md:flex-row mb-4 items-center">
    <div class="relative">
        <img :src="website.thumbnailUrl" class="w-full rounded-xl w-72 max-w-none md:w-autoz md:max-h-40"/>
        <!-- <span class="absolute px-2 py-1 text-xs text-white rounded bg-gray-700/50 right-2 bottom-2">
            7 min
        </span> -->
    </div>
    <div class="flex flex-col justify-between items-center">
        <div class="flex flex-col justify-center my-2 text-gray-700 dark:text-white md:m-0">
            <p class="text-sm text-left">
                <span class="py-0.5 mr-1">{{new Date(website.date).toLocaleDateString()}}</span> <span class="px-2 py-0.5 rounded-full" :class="website.status ? 'bg-green-500/80' : 'bg-red-500/80'">{{website.status ? 'Active' : 'Inactive'}}</span>
            </p>
            <p class="text-2xl text-left">
                {{website.title}}
            </p>
            <p class="text-sm text-left font-normal mb-2">
                {{website.body}}
            </p>
            <a :href="website.url" class="text-sm text-left font-normal  text-blue-400 underline pb-3">
                {{website.url}}
            </a>
            <div class="text-sm text-left flex">
                <div class="px-2 py-0.5 rounded-full bg-orange-500/80">Edit website</div>
            </div>
        </div>
    </div>
</div>

</div>
</template>

<script>
import { ref, watch, defineComponent } from 'vue'  
export default defineComponent({
  props: {
    msg: {
      type: String,
      required: false,
    },
  },
  async created() {
    const response = await fetch("https://run.mocky.io/v3/c5f76519-3cde-4fe7-9124-a8f1dd91d413");
    const data = await response.json();
    this.websites = data.data;
  },
  data() {
      return {
          websites: {},
      }
  }
})
</script>

<style scoped>
</style>
