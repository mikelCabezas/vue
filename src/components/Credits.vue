
<template>
<div class="h-full">
  <div class="p-4 bg-white shadow-lg rounded-2xl w-full h-full  dark:bg-indigo-800 flex flex-col items-center justify-center">
      <div class="flex items-center">
          <p class="text-gray-700 text-md text-bold dark:text-gray-50 text-center w-full">Total credits</p>
      </div>
      <div class="flex flex-col justify-start w-2/3 mx-auto">
          <p class="mt-1 mb-4 text-7xl font-bold text-center text-gray-800 dark:text-white">
              {{credits.totalCredits}}
          </p>
          <div class="relative h-2 bg-gray-200 rounded w-full">
              <div class="absolute top-0 left-0 h-2 bg-green-500 rounded" :class="`w-[${(credits.usedCredits / credits?.totalCredits) * 100}%]`"> 
              </div>
          </div>
          <div class="text-center mt-3"><strong>Remaining credits:</strong> 
          <p class="text-4xl font-bold text-center text-gray-800 dark:text-white">
              {{credits.availableCredits}}
          </p></div>
          <div class="text-center mt-3"><strong>Expires in:</strong>
            <p class="text-sm text-center text-gray-800 dark:text-white">{{new Date(credits.expires).toLocaleDateString('es')}}</p>
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
  const response = await fetch("https://run.mocky.io/v3/559909d5-f764-4641-9d79-9509e418107b");
  const data = await response.json();
  this.credits = data;
  console.log(data)
},
data() {
    return {
        credits: {},
        usedCreditsPercentage: `w-[${Math.round(this.credits?.usedCredits / this.credits?.totalCredits) * 100}%]`
    }
}
})
</script>

<style scoped>
</style>
